<template>
  <div class="page">
    <ul class="pagination">
      <li @click="current++&&goto(current)">
        <button style="font-size:14pt;"> 下一页</button>
      </li>
      <li @click="current--&&goto(current)">
        <button style="font-size:14pt;">上一页</button>
      </li>
      <li v-for="index in pages" @click="show(index)" :class="{'active':current==index}" :key="index">
        <button style="font-size:16pt;">{{index}}</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Page',
  data () {
    return {
      current: 1,
      showItem: 5,
      pages: []
    }
  },
  methods: {
    goto: function (current) {
      // var that = this
      if (current <= 0) {
        this.current = 1
        // eslint-disable-next-line
      }
      else {
        this.pages = []
        this.current = current
        console.log('ssss')
        // that.$parent.NextPage(current)
        console.log(this.current)
        var start = (this.current - 1) * this.showItem
        var i = 0
        while (i < this.showItem) {
          this.pages.push(i + start)
          i++
        }
      }
    },
    show: function (index) {
      this.$parent.NextPage(index)
    }
  },
  mounted () {
    var i = 0
    for (i = this.showItem; i >= 0; i--) {
      this.pages.push(i)
    }
  }
}
</script>

<style scoped>
  ul {
    height: 10%;
    width: 98%;
    margin-block-end: 0em;
    margin-block-start: 0em
  }
  .pagination li {
    list-style: none;
    float: right;
  }
  .pagination li button {
    display: 'inline-block';
    border: 0 px solid #ddd;
    background: #fff;
  }
  .pagination li.active button{
    background: #fff;
  }
</style>