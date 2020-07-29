import queue from './components/queue.vue'

<template>
  <div>
  <b-card
    title="Cam 1"
    img-src="https://i.ytimg.com/vi/oZB9PNdgMeM/maxresdefault.jpg"
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >
    <b-card-text>
     Nickname des Steuernden
    </b-card-text>

 <b-list-group flush>
      <b-list-group-item>Nickname</b-list-group-item>
      <b-list-group-item>Nickname</b-list-group-item>
      <b-list-group-item>Nickname</b-list-group-item>

    </b-list-group>

    <b-button href="#" variant="primary">Get in Line</b-button>
  </b-card>
  
</div>
</template>

<script>


export default {
  name: 'App',
 
}
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
