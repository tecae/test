<template>
  <div class="upf-container">
    <label
      v-on:drop.prevent="onFileChange"
      v-on:dragenter.prevent=";"
      v-on:dragleave.prevent=";"
      v-on:dragover.prevent=";"
      :for="id"
    >{{state.msg}}</label>
    <input type="file" v-bind="$attrs" :id="id" @change="onFileChange">
  </div>
</template>

<script>
import XLSX from "xlsx";

export default {
  name: "UploadXLSX",
  props: {
    id: { type: String, default: "uploadXLSX" },
    size: {
      type: Number,
      default: 10
    },
    getData: {
      type: Function
    }
  },
  data() {
    return {
      state: {
        msg: "Перетащите сюда файл или нажмите чтобы выбрать."
      },
      sendData: this.getData,
      sizeOn: 1048576,
      extention: ["xls", "xlsx"],
      file: null,
      parseData: null
    };
  },

  methods: {
    prevent() {},
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      this.file = files[0];
    },
    validateExtention(file) {
      let ext = /[^.]+$/.exec(file.name)[0];
      if (this.extention.indexOf(ext) === -1) {
        this.state.msg = `Недопустимый тип файла "${ext}", выберите другой файл.`;
        return false;
      }
      return true;
    },
    validateSize(file) {
      if (file.size > this.size * this.sizeOn) {
        this.state.msg = `Доступный размер файла "${
          this.size
        }MB", выберите другой файл.`;
        return false;
      }
      return true;
    },
    parseXLSX() {
      var reader = new FileReader();
      reader.onload = e => {
        var data = new Uint8Array(e.target.result);
        var workbook = XLSX.read(data, { type: "array" });
        this.parseData = Object.assign({}, workbook);
        /* DO SOMETHING WITH workbook HERE */
      };
      reader.readAsArrayBuffer(this.file);
    }
  },
  watch: {
    file: function(val, oldVal) {
      if (this.validateExtention(val) && this.validateSize(val)) {
        this.state.msg = `Файл - "${val.name}".`;
        this.parseXLSX();
      }
    },
    parseData: function(val, oldVal) {
      this.$emit("parseEnd", val);
      if(typeof this.getData === "function"){
        
        this.getData(val);
      }
      
    }
  },
  mounted() {
    /** */
  }
};
</script>

<style scoped>
.upf-container {
  position: relative;
  height: 119px;
  background: rgb(103, 253, 253);
  margin: 9px;
  padding: 9px;
}
.upf-container:hover {
  background: rgb(211, 255, 255);
}

.upf-container-bed-extension {
  background: rgb(0, 236, 236);
}

.upf-container-upload {
  background: rgb(187, 255, 255);
}

.upf-container label {
  border: 3px dashed rgba(3, 106, 209, 0.637);
  position: absolute;
  left: 6px;
  right: 6px;
  top: 6px;
  bottom: 6px;
  padding: 3em 0;
  text-align: center;
}

input[type="file"] {
  position: absolute;
  left: 6px;
  right: 6px;
  top: 6px;
  bottom: 6px;
  overflow: hidden;
  height: 50px;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
</style>
