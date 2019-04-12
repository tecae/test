<template>
  <div id="tableXLSX" v-html="dataTable"></div>
</template>
<script>
import XLSX from "xlsx";
export default {
  name: "XLSXTable",
  props: {
    params: {
      type: Object,
      default: {}
    }
  },
  data() {
    return { dataTable: null };
  },
  mounted() {
    var worksheet = this.params.Sheets[this.params.SheetNames[0]];
    this.dataTable = XLSX.utils.sheet_to_html(worksheet);
  }
};
</script>
<style scoped>

</style>
