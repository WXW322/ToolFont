<template>
  <div class="newtree">
    <chart ref="chart1" :options="orgOptions"></chart>
  </div>
</template>

<script>
export default {
  name: 'formatTree',
  data () {
    return {
      orgOptions: {}
    }
  },
  mounted () {
    this.getProTree()
  },
  watch: {
    '$route' (to, from) {
      this.getProTree()
    }
  },
  methods: {
    getProTree: function () {
      let chartS = this.$refs.chart1
      console.log(chartS)
      chartS.resize({'height': 700, 'width': 1500})
      var rPath = this.$route.path
      var length = rPath.length
      rPath = rPath.substring(1, length)
      console.log(rPath)
      let queryData = {}
      if(rPath === 'icsProFormat'){
        queryData['protocolType'] = 'icsPro'
      }
      else if(rPath === 'textProFormat'){
        queryData['protocolType'] = 'textPro'
      }
      else{
        queryData['protocolType'] = 'binaryPro'
      }
      var that = this
      that.value = 1
      this.$axios({
        url: 'http://192.168.199.129:8000/formattree/',
        method: 'POST',
        contentType: 'application/json',
        dataType: 'json',
        data: JSON.stringify(queryData)
      })
        .then(function (response) {
          let resDatas = response.data
          console.log(resDatas)
          that.orgOptions = {
            tooltip: {
              trigger: 'item',
              triggerOn: 'mousemove'
            },
            series: [
              {
                type: 'tree',
                data: [resDatas],
                top: '1%',
                left: '7%',
                bottom: '1%',
                right: '20%',
                symbolSize: 10,
                label: {
                  normal: {
                    position: 'left',
                    verticalAlign: 'middle',
                    align: 'right',
                    fontSize: 12
                  }
                },

                leaves: {
                  label: {
                    normal: {
                      position: 'right',
                      verticalAlign: 'middle',
                      align: 'left'
                    }
                  }
                },
                expandAndCollapse: true,
                animationDuration: 550,
                animationDurationUpdate: 750
              }
            ]
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style>
newtree {
  width: 1263px;
  height: 800px
}
</style>