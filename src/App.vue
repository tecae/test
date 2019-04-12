<template>
  <div id="parent">
    <!--  :getData="updateParseData" -->
    <UploadXLSX
      id="someid"
      name="someName"
      v-on:parseEnd="updateParseData"
      :size="10"
    />
    
    <XLSXTable v-if="isParseData" :params="parseData" />
  </div>
</template>

<script>
import UploadXLSX from "./components/UploadXLSX.vue";
import XLSXTable from "./components/XLSXTable.vue"

export default {
  name: "app",
  components: {
    UploadXLSX,
    XLSXTable
  },
  data:function(){
    return{
      isParseData:false,
      parseData:null
    }
  },
  methods: {
    updateParseData(data) {
      this.parseData = Object.assign({}, data);
      this.isParseData = true;
      console.log(this.isParseData, this.parseData);
    }
  }
};
</script>

<style>
#parent {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
