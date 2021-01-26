<template>
    <button :class="btnClasses">
        <slot></slot>
    </button>
</template>
<script>
import { toRefs, computed } from 'vue'
export default {
  props: {
    color: {
      type: String,
      required: false,
      default: null,
      validator: function (value) {
        return ['primary', 'secondary', 'success', 'info', 'warning', 'error'].indexOf(value) !== -1
      }
    },
    size: {
      type: String,
      required: false,
      default: 'default',
      validator: function (value) {
        return ['big', 'extra', 'small', 'default'].indexOf(value) !== -1
      }
    }
  },
  setup (props) {
    const { color, size } = toRefs(props)
    const btnClasses = computed(() => {
      return {
        // eslint-disable-next-line
        'btn': true,
        // eslint-disable-next-line
        [`btn--${color.value}`]: color.value,
        // eslint-disable-next-line
        [`btn--${size.value}`]: size.value
      }
    })
    return {
      // eslint-disable-next-line
      color,
      // eslint-disable-next-line
      size,
      btnClasses
    }
  }
}
</script>
