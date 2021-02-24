<template>
  <!-- Example Usage
  <Alert icon="fas fa-check-circle outline" animate outline closeable color="error">
    Aenean imperdiet. Quisque id odio. Cras dapibus. Pellentesque ut neque.
    Cras dapibus. Vivamus consectetuer hendrerit lacus. Sed mollis, eros et
  </Alert> -->
  <transition :name="alertAnimation">
    <div v-if="show" :class="alertClasses">
      <i v-if="icon" :class="iconClasses"></i>
      <div class="alert__content"><slot></slot></div>
      <i
        v-if="closeable"
        @click="close"
        class="cursor-pointer alert__close fas fa-times"
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
    const { outline, color, closeable, icon } = toRefs(props);
    const alertClasses = computed(() => {
      return {
        alert: true,
        [`alert--${color.value}`]: color.value,
        [`alert--outline--${color.value}`]: outline.value,
      };
    });
    const iconClasses = computed(() => {
      return {
        alert__content__pre: true,
        [icon.value]: icon.value
      }
    })
    const alertAnimation = computed(() => {
        return closeable.value && "slide-fade"
    });
    const close = () => {
      show.value = false;
    };
    return {
      iconClasses,
      alertAnimation,
      alertClasses,
      show,
      close,
    };
  },
};
</script>
