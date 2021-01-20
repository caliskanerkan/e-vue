<template>
  <div>
    test
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  setup () {
    const user = ref('Erkan')
    return {
      user
    }
  }
}
</script>
