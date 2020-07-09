<template>
  <div>
    <b-button v-b-toggle.sidebar-1> <span class="glyphicon glyphicon-menu-burger" aria-hidden="true">menu</span></b-button>
    <b-sidebar id="sidebar-1" title="Sidebar" shadow>
      <div class="px-3 py-2">
        <div>
          <ul> <a href="#1">first</a> </ul>
          <ul> <a href="#2">second</a> </ul>
          <ul> <a href="#3">third</a> </ul>
          <ul> <a href="#4">fourth</a> </ul>
        </div>
        <p>
          Cras mattis consectetur purus sit amet fermentum. Cras justo odio, dapibus ac facilisis
          in, egestas eget quam. Morbi leo risus, porta ac consectetur ac, vestibulum at eros.
        </p>
        <b-img src="https://picsum.photos/500/500/?image=54" fluid thumbnail></b-img>
      </div>
    </b-sidebar>
  <div>
    <b-button class="buttonLineNavi" v-on:click = "go ('send message', $event)" >start</b-button>
    <b-button class="buttonLineNavi" v-on:click = "stop ('send message', $event)">stop</b-button>
  </div>
  </div>
  
  
</template>

<script>
export default {
  methods: {
  go: function (message, event) {
    // now we have access to the native event
    if (event) {
      event.req.send('go');
    }
    alert('your car moves forward now')
  },
  stop: function (message, event) {
    // now we have access to the native event
    if (event) {
      event.req.send('stop');
    }
    alert('your car has stopped now')
  }
}
}
</script>

<style scoped>
.like {
  margin: 5em;
  padding: 2em;
}

.button {
  margin: 2em;
  padding: 0.5em;
}

.buttonLineNavi{
  margin: 2em;
}
</style>