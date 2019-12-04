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
      detailSplitProData: null
    }
  },
  methods: {
    ProSplitDetails: function (e) {
      console.log(e)
      var that = this
      var rowIndex = e
      var queryDatas = {'rowIndex': rowIndex}
      this.$axios({
        url: 'http://192.168.2.250:8000/common/getSplitProtoDataDetail/',
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
      var queryDatas = {'pageNum': pageNum, 'pageCnt': 10, 'proType': 'icsPro'}
      this.$axios({
        url: 'http://192.168.2.250:8000/common/querySplitSummary/',
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
  mounted () {
    var that = this
    var queryDatas = {'pageNum': 0, 'pageCnt': 10}
    console.log('nonghangshabi')
    var rPath = this.$route.path
    var length = rPath.length
    rPath = rPath.substring(1, length)
    if(rPath === 'binaryProSplit'){
      queryDatas['proType'] = 'binaryPro'
    }
    this.$axios({
      url: 'http://192.168.2.250:8000/common/querySplitSummary/',
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
  background: #E4FFC7;
  text-align: left
}
textarea {
  width: 100%;
  height : 100px;
  font-size: 16px
}
table {
  width: 100%
}
</style>