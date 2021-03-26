<template>
  <div style="padding:150px">
    <!-- <e-select v-model="text" option-value="value" option-text="name" return-object multiple searchable clearable :options="options"/>
    <EButton>Test</EButton> -->
    <e-pagination v-model="page" :total="50" :visible-page="5"/>
  </div>
</template>
<script>
import EPagination from "@/components/EPagination"
// import ESelect from "@/components/ESelect.vue";
import { ref } from "vue";
export default {
  components: {
    EPagination,
    // ESelect,
  },
  setup() {
    const page = ref(1)
    // const options = ref(["erkan","ahmet","mehmet"])
    // const options = ref([{name:'Erkan', value: 5}, {name:'Ahmet', value: 3}])
    return {
      page,
      // options
    };
  },
};
</script>
