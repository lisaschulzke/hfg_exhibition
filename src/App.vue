<template>
  <div id="app">
    <Layout streamUrl="http://hal.hfg.design:50005/live-21080.mjpg">

      <template v-slot:videocomponent></template>
      <template v-slot:controlcomponent>
        <Interface></Interface>
      </template>
      <template v-slot:queuecomponent>
        <SimpleQueueComponent></SimpleQueueComponent>
        <!-- <h1>BOTTOM_RIGHT</h1> -->
      </template>

    </Layout>
  </div>
</template>

<script>
  import Interface from './components/interface.vue';
  import Layout from './submodules/base_layout/src/components/interface.vue'
  import SimpleQueueComponent from './components/SimpleQueueComponent.vue'


  export default {
    name: 'App',
    components: {
      Interface,
      Layout,
      SimpleQueueComponent,
    },
  };
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>