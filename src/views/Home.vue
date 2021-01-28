<template>
  <div style="height:100vh;display:flex;align-items:center;justify-content:center">
    <Button outline>I'am button.</Button>
  </div>
</template>

<script>
import { ref } from 'vue'
import Button from '@/components/Button'
export default {
  components: {
    Button
  },
  setup () {
    const loading = ref(false)
    const a = () => console.log('a')
    return {
      a,
      loading
    }
  }
}
</script>
