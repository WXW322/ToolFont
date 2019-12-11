<template>
  <div>
    <table border="0" cellpadding="10">
      <tr>
        <th v-for="head in protocolHeaders" :key="head.id">{{head.data}}</th>
      </tr>
      <tr v-for="proData in protocolFieldDatas" :key="proData.id" v-on:click="ProDetails($event)">
        <td v-for="item in proData.data" :key="item.id" align="center">{{item.data}}</td>
      </tr>
    </table>
    <Page></Page>
    <textarea v-model="detailProData"></textarea>>
  </div>
</template>

<script>
import Page from './SplitPage.vue'
export default {
  name: 'icsProType',
  components: {
    'Page': Page
  },
  data () {
    return {
      protocolHeaders: null,
      protocolFieldDatas: null,
      detailProData: null
    }
  },
  methods: {
    ProDetails: function (e) {
      var that = this
      var rowIndex = e.currentTarget.rowIndex
      var queryDatas = {'rowIndex': rowIndex, 'proType': 'icsPro'}
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
      var queryDatas = {'pageNum': pageNum, 'pageCnt': 20}
      this.$axios({
        url: 'http://192.168.199.129:8000/common/queryIcsFieldTypes/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryDatas)
      })
        .then(function (response) {
          console.log(response.data)
          that.protocolHeaders = response.data['header']
          that.protocolFieldDatas = response.data['datas']
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  },
  mounted () {
    console.log('in the method')
    console.log(this.$route)
    var that = this
    var queryDatas = {'pageNum': 0, 'pageCnt': 15}
    this.$axios({
      url: 'http://192.168.199.129:8000/common/queryIcsFieldTypes/',
      method: 'POST',
      contentType: 'application/json',
      dataType: 'json',
      data: JSON.stringify(queryDatas)
    })
      .then(function (response) {
        console.log(response.data)
        that.protocolHeaders = response.data['header']
        that.protocolFieldDatas = response.data['datas']
      })
      .catch(function (error) {
        console.log(error)
      })
  }
}
</script>

<style>
table {
  width: 100%;
  height: 70%
}
tr {
  background: #E4FFC7
}
textarea {
  width: 100%;
  height : 100px;
  font-size: 16px
}
</style>
