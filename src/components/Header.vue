<template>
  <div id="header">
    <div class="container">
      <div class="row center">
        <h1>Search Github Users</h1>
        <input type="text" v-model="searchName">
        <input type="button" value="Search" class="btn btn-primary" placeholder="请输入github用户名" @click="search">
      </div>
    </div>
  </div>
</template>
<script>
import PubSub from 'pubsub-js'
export default {
  name: 'Header',
  data () {
    return {
      searchName: ''   
    }
  },
  methods: {
    search ()
    {
      // 发布消息
      const searchName = this.searchName.trim()
      if (searchName) {
        PubSub.publish('search', searchName)
      } 
    }
  }
}
</script>
<style>
#header{
  height: 200px;
}
.container .center{
  text-align: center;
}
</style>
