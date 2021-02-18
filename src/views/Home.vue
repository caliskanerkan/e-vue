<template>
  <Button loading @click="onClick">Mokoko</Button>
  <Modal v-model="show">
    <div>Deneme</div>
  </Modal>
  <Alert closeable icon="fas fa-info">
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusantium id enim quod, similique libero harum aliquid nam exercitationem quos ipsam? Repellendus quae animi delectus voluptas quis ratione commodi id dolore?
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
    Button,
  },
  setup() {
    const show = ref(false);
    const onClick = () => {
      show.value = true;
    };
    return {
      onClick,
      show,
    };
  },
};
</script>
