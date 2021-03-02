<template>
  <div v-click-outside="close" :class="selectClasses" @click="toggle">
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
      <div ref="select" class="select__list" @click.stop v-if="show">
        <div
          @click="updateValue(option)"
          v-for="(option, index) in options"
          :key="index"
          :class="[selectListItemClasses, { 'select__list__item--active': activeHandler(option) }]"
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
      default: "Select"
    }
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    const {
      modelValue,
      options,
      optionValue,
      optionText,
      returnObject,
      multiple,
      label
    } = toRefs(props);

    const show = ref(false);
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
    const valueReturner = (item) => options.value.filter((opt) => opt[optionValue.value] === item)[0].name;
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
        if(!modelValue.value) return false;
        else if(firstValueOfType === "string") return modelValue.value.some(opt => option === opt)
        else {
          return modelValue.value.some(opt => option[optionValue.value] === (returnObject.value ? opt[optionValue.value] : opt))
        }
    };
    const updateValue = (option) => {
      const firstValueOfType = firstOptionOfType();
      if (firstValueOfType === "string") valueStringTypeHandler(option);
      else if (firstValueOfType === "object") valueObjectTypeHandler(option);
    };
    return {
      empty,
      select,
      show,
      selectClasses,
      selectValueClasses,
      selectArrowClasses,
      selectClearableClasses,
      selectListItemClasses,
      updateValue,
      selectLabelText,
      activeHandler,
      valueText,
      toggle,
      close
    };
  },
};
</script>
