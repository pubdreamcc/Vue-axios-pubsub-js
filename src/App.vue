<template>
  <div id="app">
    <Header/>
    <UserMain/>
  </div>
</template>

<script>
import Header from './components/Header'
import Main from './components/Main'
export default {
  name: 'App',
  components: {
    Header,
    UserMain: Main
  }
}
</script>

<style>
#app{
  position: relative;
}
</style>
