<template>
  <div id="app">
    <b-navbar type="secondary" variant="secondary" text-variant="white">
      <b-navbar-nav class="col-12 mx-auto">
        <b-nav-item class="col-6 text-left">Dictionary</b-nav-item>
        <b-nav-item class="col-6 text-right">Stored</b-nav-item>
      </b-navbar-nav>
    </b-navbar>
      <transition name="bounce">
        <remote-data></remote-data>
        <stored-data></stored-data>
      </transition>
    <footer>
      <b-jumbotron bg-variant="secondary" class="jumbotron-fluid"></b-jumbotron>
    </footer>
  </div>
</template>

<script>
import RemoteData from './components/RemoteData.vue'
import StoredData from './components/StoredData.vue'

export default {
  name: 'App',
  components: {
    RemoteData,
    StoredData
  },
  created(){
    document.title = 'Vue test for SCB';
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
.nav-link{
  color: lightgray;
}
.nav-link:hover{
  color: white;
}
</style>
