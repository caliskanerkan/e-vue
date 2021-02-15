<template>
  <Button color="primary" @click="onClick">Open</Button>
  <Modal v-model="show"></Modal>
  <Alert closeable color="primary" animate>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptate inventore ullam fuga delectus nihil. Accusantium debitis sequi natus, cum quod ratione facere recusandae dolore libero minima exercitationem architecto, tempore et?
  </Alert>
</template>
<script>
import Modal from "@/components/Modal.vue";
import Button from "@/components/Button.vue";
import Alert from "@/components/Alert.vue";
import { ref } from "vue";
export default {
  components: {
    Alert,
    Modal,
    Button
  },
  setup() {
    const show = ref(false);
    const onClick = () => {
      show.value = true;
    };
    return {
      show,
      onClick,
    };
  },
};
</script>
