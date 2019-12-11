<template>
  <div>
    <table border="0" cellpadding="10">
      <tr>
        <th align="center">id</th>
        <th align="center">timeStamp</th>
        <th align="center">source</th>
        <th align="center">destination</th>
        <th align="center">summary</th>
      </tr>
      <tr class="aa" v-for="item in protocolDatas" :key="item.id" v-on:click="ProDetails($event)">
        <td align="center">{{item.id}}</td>
        <td align="center">{{item.time}}</td>
        <td align="center">{{item.source}}</td>
        <td align="center">{{item.destination}}</td>
        <td align="center">{{item.summary}}</td>
      </tr>
    </table>
    <Page></Page>
    <textarea v-model="detailProData"></textarea>>
  </div>
</template>

<script>
import Page from './SplitPage.vue'
export default {
  name: 'protocolList',
  components: {
    'Page': Page
  },
  data () {
    return {
      protocolDatas: null,
      detailProData: null,
      proType: ''
    }
  },
  methods: {
    getSummary: function (){
      var rPath = this.$route.path
      var length = rPath.length
      rPath = rPath.substring(1, length)
      var that = this
      var queryDatas = {'pageNum': 0, 'pageCnt': 15}
      if(rPath === 'icsProInfo'){
        queryDatas['proType'] = 'icsPro'
        that.proType = 'icsPro'
      }
      else if(rPath === 'textProInfo'){
        queryDatas['proType'] = 'textPro'
        that.proType = 'textPro'
      }
      else{
        queryDatas['proType'] = 'binaryPro'
        that.proType = 'binaryPro'
      }
      this.$axios({
        url: 'http://192.168.199.129:8000/common/queryProSummary/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          console.log(response.data)
          that.protocolDatas = response.data
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    ProDetails: function (e) {
      var that = this
      var rowIndex = e.currentTarget.rowIndex
      var queryDatas = {'rowIndex': rowIndex}
      queryDatas['proType'] = this.proType
      this.$axios({
        url: 'http://192.168.199.129:8000/common/getProtoDataDetail/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          var type = response.data
          that.detailProData = type['res']
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    NextPage: function (pageNum) {
      var that = this
      var queryDatas = {'pageNum': pageNum, 'pageCnt': 15}
      queryDatas['proType'] = this.proType
      console.log(this.proType)
      this.$axios({
        url: 'http://192.168.199.129:8000/common/queryProSummary/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          console.log(response.data)
          that.protocolDatas = response.data
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  },
  watch: {
    '$route' (to, from) {
      console.log('aaato')
      console.log(to['path'])
      console.log('aaato')
      if(to['path'] === '/binaryProInfo'){
        console.log('enter')
        this.getSummary()
      }
    }
  },
  mounted () {
    this.getSummary()
  }
}
</script>

<style>
table {
  width: 100%;
  height: 70%
}
th {
  background: #D9EDF7;
  font-size: 20px;
}
td {
  background: #F9F9F9;
  font-size: 20px
}
textarea {
  width: 100%;
  height : 200px;
  font-size: 20px
}
</style>
