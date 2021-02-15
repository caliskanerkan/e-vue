<template>
  <!-- Example Usage
  <Alert icon="fas fa-check-circle outline" animate outline closeable color="error">
    Aenean imperdiet. Quisque id odio. Cras dapibus. Pellentesque ut neque.
    Cras dapibus. Vivamus consectetuer hendrerit lacus. Sed mollis, eros et
  </Alert> -->
  <transition :name="alertAnimation">
    <div v-if="show" :class="alertClasses">
      <i v-if="icon" :class="icon"></i>
      <div class="alert--content"><slot></slot></div>
      <i
        v-if="closeable"
        @click="close"
        class="cursor-pointer alert--close fas fa-times"
      ></i>
    </div>
  </transition>
</template>

<script>
import { toRefs, computed, ref } from "vue";
export default {
  props: {
    closeable: {
      required: false,
      type: Boolean,
      default: false,
    },
    color: {
      required: false,
      type: String,
      default: "primary",
      validator: function (val) {
        return (
          [
            "primary",
            "secondary",
            "success",
            "info",
            "warning",
            "error",
          ].indexOf(val) !== -1
        );
      },
    },
    animate: {
      type: Boolean,
      default: false,
      required: false
    },
    icon: {
      type: String,
      default: null,
      required: false,
    },
    outline: {
      type: Boolean,
      default: false,
      required: false,
    },
  },
  setup(props) {
    const show = ref(true);
    const { outline, color, animate } = toRefs(props);
    const alertClasses = computed(() => {
      return {
        alert: true,
        [`alert__${color.value}`]: color.value,
        [`alert__outline__${color.value}`]: outline.value,
      };
    });
    const alertAnimation = computed(() => {
        return animate.value && "slide-fade"
    });
    const close = () => {
      show.value = false;
    };
    return {
      alertAnimation,
      alertClasses,
      show,
      close,
    };
  },
};
</script>
