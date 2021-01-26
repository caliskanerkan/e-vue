<template>
  <div class="half-circle-spinner">
    <div :class="classBinder"></div>
    <div :class="classBinder"></div>
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
      default: 12,
      type: Number,
      required: false
    },
    color: {
      type: String,
      required: false,
      default: 'red'
    }
  },
  setup (props) {
    const { color, size } = toRefs(props)
    const circleColor = computed(() => {
      return `border-bottom-color:${color.value}`
    })
    const classBinder = computed(() => {

    })
    return {
      classBinder,
      circleColor,
      // eslint-disable-next-line
      color,
      // eslint-disable-next-line
      size
    }
  }
}
</script>
