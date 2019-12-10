<template>
  <div class>
    <table border="0" cellpadding="10">
      <li v-for="item in protocolSplitDatas" :key="item.id" v-on:click="ProSplitDetails(item.id)">
        {{item.data}}
      </li>
    </table>
    <Page></Page>
    <textarea v-model="detailSplitProData"></textarea>>
  </div>
</template>

<script>
import Page from './SplitPage.vue'
export default {
  name: 'protocolSplit',
  components: {
    'Page': Page
  },
  data () {
    return {
      protocolSplitDatas: null,
      detailSplitProData: null,
      proType: ''
    }
  },
  methods: {
    ProSplitMsgs: function () {
      var that = this
      var queryDatas = {'pageNum': 0, 'pageCnt': 15}
      console.log('nonghangshabi')
      var rPath = this.$route.path
      var length = rPath.length
      rPath = rPath.substring(1, length)
      console.log(rPath)
      if(rPath === 'binaryProSplit'){
        queryDatas['proType'] = 'binaryPro'
        this.proType = 'binaryPro'
      }
      else if(rPath === 'icsProSplit'){
        queryDatas['proType'] = 'icsPro'
        this.proType = 'icsPro'
      }
      else{
        queryDatas['proType'] = 'textPro'
        this.proType = 'textPro'
      }
      console.log(queryDatas)
      this.$axios({
        url: 'http://192.168.199.129:8000/common/querySplitSummary/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          console.log(response.data)
          that.protocolSplitDatas = response.data
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    ProSplitDetails: function (e) {
      console.log(e)
      var that = this
      var rowIndex = e
      var queryDatas = {'rowIndex': rowIndex}
      queryDatas['proType'] = this.proType
      this.$axios({
        url: 'http://192.168.199.129:8000/common/getSplitProtoDataDetail/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          console.log(response.data)
          var type = response.data
          that.detailSplitProData = type['res']
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    NextPage: function (pageNum) {
      var that = this
      var queryDatas = {'pageNum': pageNum, 'pageCnt': 10, 'proType': this.proType}
      this.$axios({
        url: 'http://192.168.199.129:8000/common/querySplitSummary/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          console.log(response.data)
          that.protocolSplitDatas = response.data
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  },
  watch: {
    '$route' (to, from) {
      this.ProSplitMsgs()
    }
  },
  mounted () {
    var that = this
    var queryDatas = {'pageNum': 0, 'pageCnt': 15}
    console.log('nonghangshabi')
    var rPath = this.$route.path
    var length = rPath.length
    rPath = rPath.substring(1, length)
    console.log(rPath)
    if(rPath === 'binaryProSplit'){
      queryDatas['proType'] = 'binaryPro'
      this.proType = 'binaryPro'
    }
    else if(rPath === 'icsProSplit'){
      queryDatas['proType'] = 'icsPro'
      this.proType = 'icsPro'
    }
    else{
      queryDatas['proType'] = 'textPro'
      this.proType = 'textPro'
    }
    console.log(queryDatas)
    this.$axios({
      url: 'http://192.168.199.129:8000/common/querySplitSummary/',
      method: 'POST',
      contentType: 'application/json',
      dataType: 'json',
      data: JSON.stringify(queryDatas)
    })
      .then(function (response) {
        console.log(response.data)
        that.protocolSplitDatas = response.data
      })
      .catch(function (error) {
        console.log(error)
      })
  }
}
</script>

<style scoped>
li {
  width: 100%;
  height: 20px;
  background: #F9F9F9;
  text-align: left
}
textarea {
  width: 100%;
  height : 300px;
  font-size: 16px
}
table {
  width: 100%
}
</style>