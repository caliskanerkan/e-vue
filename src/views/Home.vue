<template>
  <div style="padding:150px">
    <e-select v-model="text" option-text="ahmet" option-value="value" return-object clearable :options="options"/>
  </div>
</template>
<script>
import ESelect from "@/components/ESelect.vue";
import { ref } from "vue";
export default {
  components: {
    ESelect
  },
  setup() {
    const text = ref("")
    const options = ref([{ahmet:'Erkan', value: 5}, {ahmet:'Ahmet', value: 3}])
    return {
      text,
      options
    };
  },
};
</script>
