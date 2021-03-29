<template>
  <div class="pagination">
    <e-button @click="paginationHandler('prev')" :disabled="disabledHandler('start')" :size="size" :fab="circle" :color="color">
      <e-icon name="fas fa-chevron-left" />
    </e-button>
    <div
      @click="updateValue(page)"
      class="pagination__item"
      v-for="page in visiblePageNumbers"
      :key="page"
    >
      <e-button :size="size" :color="color" :outline="!activeHandler(page)" :fab="circle" v-text="page" />
    </div>
    <e-button @click="paginationHandler('next')" :disabled="disabledHandler('end')" :size="size" :fab="circle" :color="color">
      <e-icon name="fas fa-chevron-right" />
    </e-button>
  </div>
</template>
<script>
import { computed, toRefs } from "vue";
import EButton from "./EButton";
import EIcon from "./EIcon";
export default {
  components: { EButton, EIcon },
  emits: ["update:modelValue"],
  props: {
    size: {
      required: false,
      type: String,
      default: "small",
      validator: function (value) {
        return ["big", "extra", "small", "normal"].indexOf(value) !== -1;
      },
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
    circle:{
      required: false,
      type: Boolean,
      default: true
    },
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
  setup(props, { emit }) {
    const { modelValue, total, visiblePage } = toRefs(props);
    const updateValue = (page) => emit("update:modelValue", page);
    const activeHandler = (page) => page === modelValue.value;
    const disabledHandler = (position) => position === "start" ? modelValue.value === 1 : modelValue.value === total.value
    const paginationHandler = (type) => {
      let page = modelValue.value
      const position = type === "prev" ? "start" : "end"
      if(!disabledHandler(position)) updateValue(type === "next" ?  ++page : --page)
    }
    const visiblePageNumbers = computed(() => {
      const pageArray = Array.from({length: total.value}, (_, index) => index + 1)
      // const seperator = 3
      if(total.value <= visiblePage.value) return pageArray
      return pageArray
    })
    return {
      visiblePageNumbers,
      updateValue,
      activeHandler,
      disabledHandler,
      paginationHandler,
    };
  },
};
</script>
