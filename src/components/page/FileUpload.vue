<template>
  <form>
  <div class="row">
    <div class="col-4" id="firstOne">
      <img>
    </div>
    <div class="col-2" id="firstOne">
      <input type="file" @change="getFile($event)" />
    </div>
    <div class="col-1">
      <button @click="upload">上传</button>
    </div>
     <div class="col-2">
        <label>协议类型</label>
      <select id="proSelect" v-model="selectData">
        <option v-for="item in proList" :key="item.id">{{item.data}}</option>
      </select>
    </div>
    <div class="col-1">
      <div v-if="uping==1">正在上传中</div>
      <div v-if="uping==2">文件上传成功</div>
      <div v-if="uping==3">文件上传失败</div>
    </div>
  </div>
</form>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css'
import 'bootstrap/dist/js/bootstrap.min.js'
export default {
  name: 'FileUpload',
  data () {
    return {
      orgOptions: {},
      upath: '',
      result: '',
      uping: 0,
      selectData: '',
      proList: [{'id': 1, 'data': 'icsPro'}, {'id': 2, 'data': 'textPro'}, {'id': 3, 'data': 'binaryPro'}]
    }
  },
  methods: {
    getFile: function (event) {
      this.upath = event.target.files[0]
      console.log(this.upath.name)
      console.log(this.selectData)
    },
    upload: function () {
      var zipFormData = new FormData()
      zipFormData.append(this.upath.name, this.upath)
      zipFormData.set('fileType', this.selectData)
      zipFormData.set('fileName', this.upath.name)
      var that = this
      this.uping = 1
      this.$axios({
        url: 'http://192.168.199.129:8000/common/fileUpload/',
        method: 'POST',
        contentType: 'multipart/form-data',
        data: zipFormData
      })
        .then(function (response) {
          that.uping = 2
          console.log(response.data)
        })
        .catch(function (error) {
          that.uping = 3
          console.log(error)
        })
    }
  }
}
</script>

<style>
  #first {
    height: 40px
  }
  #bt1 {
    background: #FFFFFF;
    height: 35px
  }
  #col-1 {
    margin-right: 0px
  }
  #row {
    margin:0 auto
  }