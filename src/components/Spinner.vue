<template>
  <div class="spinner" :style="spinnerSize">
    <div class="spinner--item" :style="{'border-top-color': color}" :class="spinnerClasses"></div>
    <div class="spinner--item" :style="{'border-bottom-color': color}" :class="spinnerClasses"></div>
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
      validator: function (value) {
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
      default: 'red'
    }
  },
  setup (props) {
    const { color, size, speed } = toRefs(props)
    // const borderSize = computed(() => {
    //   return {
    //     border: `${size.value / 10}px solid transparent`
    //   }
    // })
    const spinnerClasses = computed(() => {
      return {
        [`spinner__border__${speed.value}`]: speed.value
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
      // eslint-disable-next-line
      speed,
      // eslint-disable-next-line
      color,
      // eslint-disable-next-line
      size
    }
  }
}
</script>
