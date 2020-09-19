<template>
  <div id="gets">
    <h3>{{disclaimered}}</h3>
    <section v-if="errors">
      <h3>获取数据失败</h3>
    </section>
    <div class="datas">
      <ul>
        <li>{{timer.updated}}</li>
        <li>{{timer.updatedISO}}</li>
        <li>{{timer.updateduk}}</li>
    </ul>
    </div>
    <div v-for="item in info" :key="item.index"  class="item">
      {{item.description}}:
      <span class="rate_float_span">
        <span v-html="item.symbol"></span>{{item.rate_float | itemdecimal}}
      </span>
    </div>
  </div>
</template>

<script>
const axios = require('axios');

export default {
  name:'getData',
  data() {
    return {
      disclaimered:null,
      timer:{},     
      info:null,
      errors:false
    }
  },
  filters: {
    itemdecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted() {
    axios
    .get('https://api.coindesk.com/v1/bpi/currentprice.json')
    .then(response => {
      this.disclaimered = response.data.disclaimer
      this.timer = response.data.time
      this.info = response.data.bpi      
    })
    .catch(error => {
      this.errors = true
    })
  }
}
</script>

<style>

</style>