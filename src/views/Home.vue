<template>
  <div style="text-align:center;margin-top:10px">
    <Button color="primary"> <Spinner :size="13" speed="fast" color="pink" /></Button>
  </div>
</template>

<script>
import Button from '@/components/Button'
import Spinner from '@/components/Spinner'
import { ref } from 'vue'
export default {
  components: {
    Spinner,
    Button
  },
  setup () {
    const user = ref('Erkan')
    return {
      user
    }
  }
}
</script>
