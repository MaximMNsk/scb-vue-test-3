<template>
  <div id="app">
    <b-navbar type="secondary" variant="secondary" text-variant="white">
      <b-navbar-nav class="col-12 mx-auto">
        <b-nav-item class="col-6 text-left" @click="chooseFrame('dict')">Dictionary</b-nav-item>
        <b-nav-item class="col-6 text-right" @click="chooseFrame('stored')">Stored</b-nav-item>
      </b-navbar-nav>
    </b-navbar>
      <transition name="bounce">
        <remote-data v-if="isRemoteSearch" :searchDirection="isRemoteSearch"></remote-data>
        <stored-data v-else></stored-data>
      </transition>
    <footer>
      <b-jumbotron bg-variant="secondary" class="jumbotron-fluid"></b-jumbotron>
    </footer>
  </div>
</template>

<script>
  import RemoteData from './components/RemoteData.vue';
  import StoredData from './components/StoredData.vue';

  export default {
    name: 'App',
    components: {
      RemoteData,
      StoredData
    },
    data() {
      return{
        isRemoteSearch: true
      }
    },
    methods: {
      chooseFrame(way){
        return this.isRemoteSearch = (way==='stored') ? false : true;
      }
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
  .bounce-enter-active {
    animation: bounce-in .5s;
    display: none;
  }
  .bounce-leave-active {
      animation: bounce-in .5s reverse;
      display: none;
  }
  @keyframes bounce-in {
    0% {
        transform: scale(0);
    }
    100% {
        transform: scale(1);
    }
  }
</style>
