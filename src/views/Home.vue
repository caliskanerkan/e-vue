<template>
  <Button color="success" :loading="loading" :disabled="loading" size="extra" icon="fas fa-home"></Button>
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
