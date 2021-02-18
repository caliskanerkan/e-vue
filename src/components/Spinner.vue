<template>
  <div class="spinner" :style="spinnerSize">
    <div
      :style="{ 'border-top-color': color }"
      :class="spinnerClasses"
    ></div>
    <div
      :style="{ 'border-bottom-color': color }"
      :class="spinnerClasses"
    ></div>
  </div>
</template>
<script>
import { toRefs, computed } from 'vue'
export default {
  props: {
    speed: {
      default: 'normal',
      type: String,
      required: false,
      validator: function(value) {
        return ['fast', 'slow', 'normal'].indexOf(value) !== -1
      }
    },
    size: {
      default: 20,
      type: Number,
      required: true
    },
    color: {
      type: String,
      required: false,
      default: '#fff'
    }
  },
  setup(props) {
    const { size, speed } = toRefs(props)

    const spinnerClasses = computed(() => {
      return {
        spinner__item: true,
        [`spinner--border--${speed.value}`]: speed.value
      }
    })
    const spinnerSize = computed(() => {
      return {
        width: `${size.value}px`,
        height: `${size.value}px`
      }
    })
    return {
      spinnerClasses,
      spinnerSize,
    }
  }
}
</script>
