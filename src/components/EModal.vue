<template>
  <teleport to="body">
    <transition
      enter-active-class="animate__animated animate__faster animate__fadeIn"
      enter-class="animate__animated animate__faster animate__fadeIn"
      leave-active-class="animate__animated animate__faster animate__fadeOut"
      leave-class="animate__animated animate__faster animate__fadeOut"
      appear
    >
      <div class="modal__overlay" v-if="modelValue" @click="close">
        
      </div>
    </transition>
    <transition
      appear
      enter-active-class="animate__animated animate__faster animate__zoomIn"
      enter-class="animate__animated animate__faster animate__zoomIn"
      leave-active-class="animate__animated animate__faster animate__zoomOut"
      leave-class="animate__animated animate__faster animate__zoomOut"
    >
      <div
        ref="modal"
        :class="modalClasses"
        @click.stop
        role="dialog"
        v-if="modelValue"
      >
        <i :class="closeClasses" @click="close"></i>
        <slot></slot>
      </div>
    </transition>
  </teleport>
</template>
<script>
import { toRefs, watch, computed, ref, watchEffect } from "vue";
export default {
  props: {
    modelValue: {
      required: true,
      type: Boolean,
      default: false,
    },
    fullscreen: {
      required: false,
      default: false,
      type: Boolean,
    },
    size: {
      required: false,
      default: "md",
      type: String,
      validator: function(value) {
        return ["sm","md","lg","xl"].indexOf(value) !== -1
      }
    },
    persist: {
      default: false,
      type: Boolean,
      required: false
    }
  },
  emits: ["update:modelValue"],
  setup(props, context) {
    const modal = ref(null);
    const { modelValue, size, fullscreen } = toRefs(props);
    const close = () => {
      context.emit("update:modelValue", false);
    };
    const open = () => {
      context.emit("update:modelValue", true)
    }
    const modalClasses = computed(() => {
      return {
        modal: true,
        "box-shadow": true,
        [`modal--${size.value}`]: size.value,
        "modal--fullscreen": fullscreen.value
      };
    });
    const closeClasses = computed(() => {
      return {
        "modal__close": true,
        "fas fa-times": true,
        hide: fullscreen.value
      }
    })
    const keyboardEscape = ({ keyCode }) => {
      if (keyCode === 27) close();
    };
    watch(modelValue, (modelValue) => {
      modelValue
        ? (document.documentElement.style.overflow = "hidden")
        : (document.documentElement.style.overflow = "auto");
    });
    watchEffect(
      () => {
        if (modal.value) document.addEventListener("keyup", keyboardEscape);
        else document.removeEventListener("keyup", keyboardEscape);
      },
      { flush: "post" }
    );
    return {
      open,
      modal,
      close,
      closeClasses,
      modalClasses,
    };
  },
};
</script>
