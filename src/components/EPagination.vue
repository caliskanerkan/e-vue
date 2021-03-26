<template>
  <div class="pagination">
    <div
      @click="updateValue(page)"
      class="pagination__item"
      v-for="page in visiblePage"
      :key="page"
    >
      <e-button :outline="!activeHandler(page)" fab v-text="page" />
    </div>
  </div>
</template>
<script>
import { toRefs } from "vue";
import EButton from "./EButton";
export default {
  components: { EButton },
  emits: ["update:modelValue"],
  props: {
    modelValue: {
      required: true,
      type: Number,
    },
    total: {
      required: true,
      type: Number,
    },
    visiblePage: {
      required: false,
      type: Number,
      default: 5,
    },
  },
  setup(_, { emit }) {
    const { modelValue } = toRefs(_);
    const updateValue = (page) => emit("update:modelValue", page);
    const activeHandler = (page) => page === modelValue.value;
    return {
      activeHandler,
      updateValue,
    };
  },
};
</script>
