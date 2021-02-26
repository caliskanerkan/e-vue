<template>
  <div v-click-outside="close" :class="selectClasses" @click="toggle">
    <div :class="selectValueClasses">
      <div class="select__label">
        <div>{{ value }}</div>
      </div>
      <div class="select__icon__container">
        <i
          v-if="clearable && modelValue"
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
      <div ref="select" class="select__list" @click.stop v-if="show">
        <div class="select__search" v-if="searchable">
          <input
            :placeholder="qPlaceholder"
            type="text"
            :value="q"
            @input="(e) => (q = e.target.value)"
          />
        </div>
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
import { ref, computed, toRefs } from "vue";
export default {
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
    optionValue: {
      required: false,
      type: String,
      default: "",
    },
    label: {
      type: String,
      default: "Select",
      required: false,
    },
    searchable: {
      type: Boolean,
      required: false,
      default: false,
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
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    const {
      label,
      modelValue,
      options,
      optionValue,
      optionText,
      returnObject,
      multiple,
    } = toRefs(props);

    const show = ref(false);
    const q = ref("");
    const select = ref();

    const selectClasses = computed(() => {
      return {
        select: true,
      };
    });
    const selectValueClasses = computed(() => {
      return {
        select__value: true,
        "select--focus": show.value,
      };
    });
    const selectArrowClasses = computed(() => {
      return {
        select__icon: true,
        "fas fa-chevron-down": true,
        select__arrow: true,
        [show.value ? "select__arrow--down" : "select__arrow--up"]: true,
      };
    });
    const selectClearableClasses = computed(() => {
      return {
        select__icon: true,
        "fas fa-times": true,
      };
    });
    const selectListItemClasses = computed(() => {
      return {
        select__list__item: true,
      };
    });
    const value = computed(() => {
      return modelValue.value ? modelValue.value : label.value;
    });
    const qPlaceholder = computed(() => `${label.value} Search`);
    const searchableOptions = computed(() => {
      if (!q.value) return options.value;
    });
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
    const activeHandler = (option) => modelValue.value === option;
    const updateValue = (option) => {
      const [firstValue] = options.value;
      const firstValueOfType = typeof firstValue;
      if (firstValueOfType === "string") valueStringTypeHandler(option);
      else if (firstValueOfType === "object") valueObjectTypeHandler(option);
    };
    return {
      q,
      select,
      show,
      selectClasses,
      selectValueClasses,
      selectArrowClasses,
      selectClearableClasses,
      selectListItemClasses,
      searchableOptions,
      qPlaceholder,
      updateValue,
      selectLabelText,
      activeHandler,
      value,
      toggle,
      close,
    };
  },
};
</script>
