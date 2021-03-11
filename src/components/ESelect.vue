<template>
  <div v-click-outside="close" class="select" @click="toggle">
    <div :class="selectValueClasses">
      <div class="select__label">
        <div>{{ valueText }}</div>
      </div>
      <div class="select__icon__container">
        <i
          v-if="clearable && !empty"
          @click.stop
          @click="$emit('update:modelValue', '')"
          :class="selectClearableClasses"
        ></i>
        <i :class="selectArrowClasses"></i>
      </div>
    </div>
    <transition
      appear
      enter-active-class="animate__animated animate__faster animate__fadeIn"
      enter-class="animate__animated animate__faster animate__fadeIn"
      leave-active-class="animate__animated animate__faster animate__fadeOut"
      leave-class="animate__animated animate__faster animate__fadeOut"
    >
      <div class="select__list" @click.stop v-if="show">
        <div class="select__search" v-if="searchable">
          <input
            ref="searchRef"
            type="text"
            placeholder="Search..."
            v-model="search"
          />
        </div>
        <div
          class="select__search--no-data"
          v-if="searchableOptions.length === 0"
          v-text="'No results found.'"
        ></div>
        <div
          @click="updateValue(option)"
          v-for="(option, index) in searchableOptions"
          :key="index"
          :class="[
            selectListItemClasses,
            { 'select__list__item--active': activeHandler(option) },
          ]"
        >
          {{ selectLabelText(option) }}
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
import { ref, computed, toRefs, watchEffect } from "vue";
export default {
  // Will make refactor
  props: {
    options: {
      required: true,
      type: Array,
    },
    returnObject: {
      required: false,
      type: Boolean,
      default: false,
    },
    optionText: {
      required: false,
      type: String,
      default: "",
    },
    icon: {
      default: "fas fa-chevron-down",
      required: false,
      type: String,
    },
    optionValue: {
      required: false,
      type: String,
      default: "",
    },
    multiple: {
      type: Boolean,
      required: false,
      default: false,
    },
    modelValue: {
      required: true,
      type: [String, Array, Object, Number],
    },
    clearable: {
      type: Boolean,
      required: false,
      default: false,
    },
    label: {
      type: String,
      required: false,
      default: "Select",
    },
    searchable: {
      type: Boolean,
      required: false,
      default: false,
    },
    clearIcon: {
      type: String,
      required: false,
      default: "fas fa-times",
    },
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    const {
      modelValue,
      options,
      optionValue,
      optionText,
      returnObject,
      searchable,
      multiple,
      label,
      clearIcon,
      icon,
    } = toRefs(props);

    const show = ref(false);
    const search = ref("");
    const searchRef = ref(null);
    const selectValueClasses = computed(() => {
      return {
        select__value: true,
        "select--focus": show.value,
      };
    });
    const selectArrowClasses = computed(() => {
      return {
        select__icon: true,
        [icon.value]: true,
        select__arrow: true,
        [show.value ? "select__arrow--down" : "select__arrow--up"]: true,
      };
    });
    const selectClearableClasses = computed(() => {
      return {
        select__icon: true,
        [clearIcon.value]: true,
      };
    });
    const selectListItemClasses = computed(() => {
      return {
        select__list__item: true,
      };
    });
    const empty = computed(
      () =>
        !modelValue.value ||
        (Array.isArray(modelValue.value) && modelValue.value.length === 0)
    );
    const valueText = computed(() => {
      const type = firstOptionOfType();
      if (empty.value) return `${label.value}`;
      if (type === "string") {
        if (multiple.value) return multipleValue(type);
        else return modelValue.value;
      } else {
        if (multiple.value) return multipleValue(type);
        else
          return returnObject.value
            ? modelValue.value[optionText.value]
            : valueReturner(modelValue.value);
      }
    });
    const multipleValue = (type) => {
      let _value = "";
      if (type === "string") {
        modelValue.value.map((item, index) => {
          _value += index !== modelValue.value.length - 1 ? `${item},` : item;
        });
      } else {
        modelValue.value.map((item, index) => {
          if (index !== modelValue.value.length - 1) {
            _value += returnObject.value
              ? `${item[optionText.value]},`
              : `${valueReturner(item)},`;
          } else {
            _value += returnObject.value
              ? `${item[optionText.value]}`
              : valueReturner(item);
          }
        });
      }
      return _value;
    };
    const valueReturner = (item) =>
      options.value.filter((opt) => opt[optionValue.value] === item)[0].name;
    const selectLabelText = (option) => {
      if (optionText.value) return option[optionText.value];
      else return option;
    };
    const close = () => {
      show.value = false;
    };
    const toggle = () => {
      show.value = !show.value;
    };
    const valueStringTypeHandler = (option) => {
      if (multiple.value) {
        const index = modelValue.value.indexOf(option);
        index >= 0
          ? emit(
              "update:modelValue",
              modelValue.value.filter((v) => v !== option)
            )
          : emit("update:modelValue", [...modelValue.value, option]);
      } else emit("update:modelValue", option);
    };
    const valueObjectTypeHandler = (option) => {
      const key = optionValue.value;
      if (multiple.value) {
        if (modelValue.value.length === 0)
          emit(
            "update:modelValue",
            returnObject.value ? [option] : [option[optionValue.value]]
          );
        else {
          const index = modelValue.value.findIndex((val) =>
            returnObject.value ? val[key] === option[key] : val === option[key]
          );
          index >= 0
            ? emit(
                "update:modelValue",
                modelValue.value.filter((val) =>
                  returnObject.value
                    ? val[key] !== option[key]
                    : val !== option[key]
                )
              )
            : emit("update:modelValue", [
                ...modelValue.value,
                returnObject.value ? option : option[key],
              ]);
        }
      } else
        return emit(
          "update:modelValue",
          returnObject.value ? option : option[key]
        );
    };
    const firstOptionOfType = () => {
      const [firstValue] = options.value;
      return typeof firstValue;
    };
    const activeHandler = (option) => {
      const firstValueOfType = firstOptionOfType();
      if (!modelValue.value) return false;
      else if (firstValueOfType === "string") {
        return !multiple.value ? modelValue.value === option : modelValue.value.some(opt => opt === option);
      } else {
        if(multiple.value) return returnObject.value ? modelValue.value.some(opt => opt === option) : modelValue.value.some(opt => opt[optionValue.value] === option[optionValue.value])
        else return returnObject.value ? modelValue.value === option : modelValue.value === option[optionValue.value]
      }
    };
    const updateValue = (option) => {
      const firstValueOfType = firstOptionOfType();
      if (firstValueOfType === "string") valueStringTypeHandler(option);
      else if (firstValueOfType === "object") valueObjectTypeHandler(option);
    };
    const searchableOptions = computed(() => {
      if (!searchable.value || !search.value) return options.value;
      const valueType = firstOptionOfType();
      if (valueType === "string")
        return options.value.filter(
          (opt) =>
            opt.toLowerCase().indexOf(search.value.toLowerCase().value) >= 0
        );
      else
        return options.value.filter(
          (opt) =>
            opt[optionText.value]
              .toLowerCase()
              .indexOf(search.value.toLowerCase()) >= 0
        );
    });

    watchEffect(
      () => {
        show.value && searchable.value && searchRef.value.focus();
      },
      { flush: "post" }
    );
    return {
      searchableOptions,
      empty,
      search,
      searchRef,
      show,
      selectValueClasses,
      selectArrowClasses,
      selectClearableClasses,
      selectListItemClasses,
      updateValue,
      selectLabelText,
      activeHandler,
      valueText,
      toggle,
      close,
    };
  },
};
</script>
