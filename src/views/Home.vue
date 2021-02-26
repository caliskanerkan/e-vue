<template>
  <div style="padding:150px">
    <e-select v-model="text" clearable option-text="name" option-value="value" multiple return-object :options="options"/>
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
    // const options = ref(["erkan","ahmet","mehmet"])
    const options = ref([{name:'Erkan', value: 5}, {name:'Ahmet', value: 3}])
    return {
      text,
      options
    };
  },
};
</script>
