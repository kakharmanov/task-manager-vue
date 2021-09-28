<template>
  <div id="app">
    <my-navbar></my-navbar>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<script>
import MyNavbar from './components/MyNavbar'
export default {
  components:{
    MyNavbar
  }
}
</script>>


<style lang="scss">
@import "~materialize-css/dist/css/materialize.min.css";


</style>
