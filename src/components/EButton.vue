<template>
  <button :class="btnClasses" :style="iconSizer" :disabled="disabled">
    <span class="btn__content" :style="{ opacity: btnLabelOpacity }">
      <slot></slot>
    </span>
    <span v-if="loading" class="btn__loading">
      <e-spinner speed="slow" :size="20" :color="spinnerColor" />
    </span>
  </button>
</template>
<script>
import { toRefs, computed } from "vue";
import { colors } from "@/utils/color"
import ESpinner from "./ESpinner.vue";
export default {
  // when button outline spinner color fix
  components: { ESpinner },
  props: {
    outline: {
      type: Boolean,
      required: false,
      default: false,
    },
    block: {
      type: Boolean,
      required: false,
      default: false,
    },
    color: {
      type: String,
      required: false,
      default: "primary",
      validator: function (value) {
        return (
          [
            "primary",
            "secondary",
            "success",
            "info",
            "warning",
            "error",
          ].indexOf(value) !== -1
        );
      },
    },
    loading: {
      type: Boolean,
      required: false,
      default: false,
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false,
    },
    round: {
      type: Boolean,
      required: false,
    },
    size: {
      type: String,
      required: false,
      default: "normal",
      validator: function (value) {
        return ["big", "extra", "small", "normal"].indexOf(value) !== -1;
      },
    },
    fab: {
      required: false,
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const {
      color,
      size,
      disabled,
      loading,
      round,
      block,
      fab,
      outline,
    } = toRefs(props);
    const type = {
      big: 50,
      extra: 60,
      normal: 40,
      small: 30,
    };
    const iconSizer = computed(() => {
      if (fab.value) {
        return {
          height: `${type[size.value]}px !important`,
          width: `${type[size.value]}px !important`,
          borderRadius: "50%",
        };
      }
    });
    const btnLabelOpacity = computed(() => (loading.value ? 0 : 1));
    const btnClasses = computed(() => {
      return {
        btn: true,
        [`btn--${color.value}`]: color.value,
        [`btn--${size.value}`]: size.value,
        "btn--round": round.value,
        "btn--block": block.value,
        "btn--disable": disabled.value,
        [`btn--outline--${color.value}`]: outline.value,
      };
    });
    const spinnerColor = computed(() => !outline.value ? '#fff' : colors[color.value])
    return {
      spinnerColor,
      iconSizer,
      btnLabelOpacity,
      btnClasses
    };
  },
};
</script>
