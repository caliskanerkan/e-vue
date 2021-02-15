<template>
  <teleport to="body">
    <transition
      name="animate__animated animate__animated__faster"
      enter-active-class="animate__fadeIn"
      enter-class="animate__fadeIn"
      leave-active-class="animate__fadeOut"
      leave-class="animate__fadeOut"
      appear
    >
      <div class="modal--overlay" v-if="modelValue" @click="close"></div>
    </transition>
    <transition
      appear
      name="animate__animated animate__animated__faster"
      enter-active-class="animate__zoomIn"
      enter-class="animate__zoomIn"
      leave-active-class="animate__zoomOut"
      leave-class="animate__zoomOut"
    >
      <div class="modal box-shadow" @click.stop role="dialog" v-if="modelValue">
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Obcaecati rerum
        qui tenetur! Neque, iste? Nesciunt nostrum, eum eligendi assumenda
        exercitationem, voluptate laudantium provident modi, earum voluptas
        consequuntur? Odit, sequi cupiditate?
      </div>
    </transition>
  </teleport>
</template>
<script>
import { toRefs, watch } from "vue";
export default {
  props: {
    modelValue: {
      required: true,
      type: Boolean,
      default: false,
    },
  },
  emits: ["update:modelValue"],
  setup(props, context) {
    const { modelValue } = toRefs(props);
    const close = () => {
      context.emit("update:modelValue", false);
    };
    watch(modelValue, (modelValue) => {
      modelValue
        ? (document.documentElement.style.overflow = "hidden")
        : (document.documentElement.style.overflow = "auto");
    });
    return {
      close,
    };
  },
};
</script>
