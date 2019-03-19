<template>
  <div id="main">
    <h1 v-if="firstview">请输入搜索用户的名称</h1>
    <h1 v-if="loading">loading</h1>
    <h1 v-if="errormsg">{{errormsg}}</h1>
      <div class="row">
        <div class="col-xs-6 col-md-3" v-for="(user,index) in users" :key="index">
          <a :href="user.url">
            <img :src="user.avatar_url">
          </a>
          <span>{{user.name}}</span>
        </div>
      </div>
  </div>
</template>
<script>
import PubSub from 'pubsub-js'
import axios from 'axios'
// https://api.github.com/search/users?q=
export default {
  name: 'Main',
  data () {
    return {
      firstview: true,
      loading: false,
      errormsg: '',
      users: null // [{url:' ',name:'',avatar_url:""}]
    }
  },
  mounted () {
    // 订阅消息
    PubSub.subscribe('search', (msg, searchName) => {
      const url = 'https://api.github.com/search/users?q='+searchName
      // 更新状态
      this.firstview = false
      this.loading = true
      this.users=null
      this.errormsg=''
      axios.get(url).then((response) => {
        const result = response.data
        const users = result.items.map(item => ({
          url: item.html_url,
          avatar_url: item.avatar_url,
          name: item.login 
        }))
        // 更新请求成功的状态
        this.loading=false
        this.users=users
      }).catch(error => {
        this.loading=false
        this.errormsg='搜索失败'
      })
    })
  }
}
</script>
<style>
#main > .row ,#main h1{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
#main > .row > div{
  float: left;
  width: 100px;
  padding: 0;
  text-align: center;
  color: red;
  font-size: 18px;
  margin: 0 10px;
}
.row a img{
  width: 100px;
  height: 100px;
}
</style>
