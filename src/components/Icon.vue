<template>
  <i :class="name" :style="{ 'font-size': `${size}px`, color }"></i>
</template>

<script>
export default {
  props: {
    name: {
      required: true,
      type: String
    },
    size: {
      type: Number,
      required: false,
      default: 14
    },
    color: {
      type: String,
      required: false,
      default: '#fff'
    }
  }
}
</script>
