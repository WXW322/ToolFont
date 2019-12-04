<template>
  <div>
    <table border="0" cellpadding="10">
      <tr>
        <th>id</th>
        <th>timeStamp</th>
        <th>source</th>
        <th>destination</th>
        <th>summary</th>
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
      detailProData: null
    }
  },
  methods: {
    ProDetails: function (e) {
      var that = this
      var rowIndex = e.currentTarget.rowIndex
      var queryDatas = {'rowIndex': rowIndex}
      this.$axios({
        url: 'http://192.168.2.250:8000/common/getProtoDataDetail/',
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
      console.log('enter')
      this.$axios({
        url: 'http://192.168.2.250:8000/common/queryProSummary/',
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
  mounted () {
    console.log('in the method')
    console.log(this.$route)
    var that = this
    var queryDatas = {'pageNum': 0, 'pageCnt': 10}
    this.$axios({
      url: 'http://192.168.2.250:8000/common/queryProSummary/',
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
