<template>
  <div>
    <b-button v-b-toggle.sidebar-1> <span class="glyphicon glyphicon-menu-burger" aria-hidden="true">menu</span>
    </b-button>
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
      <div id="controlscontainer">
        <strong>Connected: {{connected}}</strong><br><br><br>
        <div class="half">
          <h4>Presets</h4>
          <button v-on:click="go()" :disabled="!connected || !amIActive">GO</button>
          <button v-on:click="stop()" :disabled="!connected || !amIActive">STOP</button><br>
          <!-- <b-button class="buttonLineNavi" v-on:click = "go ('send message', $event)" >start</b-button> -->
          <!-- <b-button class="buttonLineNavi" v-on:click = "stop ('send message', $event)">stop</b-button> -->
        </div>
        <div class="half">
          <h4>Current Waiting Queue {{this.currentTimer}}</h4>
          <SimpleQueue :currentQueue="currentQueue" :ownId="ownId" :ownName="clientName"></SimpleQueue>
        </div>
      </div>
    </div>
  </div>


</template>

<script>
  import SimpleQueue from "./SimpleQueueComponent"

    export default {
        components: {
            SimpleQueue
        },
        methods: {
            //CHANGEME: die Namen der Nachrichten die ihr mit emit() verschickt müssen mit dem Backend matchen
            buttonClick: function (preset) {
                console.log("Click button " + preset);
                this.$socket.emit('preset', preset);
            },
            go: function () {
                this.$socket.emit('move', 'forward');
            },
            stop: function () {
                this.$socket.emit('move', 'stop');
            }
        },
        sockets: {
            connect: function () {
                console.log('socket connected')
                this.connected = true;
                this.ownId = this.$socket.id
                this.$socket.emit("register_front")
            },
            disconnect: function () {
                console.log('socket disconnected')
                this.connected = false;
            },
            //CHANGEME: die Namen der Nachrichten hier reinkommen müssen mit dem Backend matchen (Funktionsname = Nachrichtenname)
            nsp_list: function (data) {
                console.log("NSPs:" + data);
            },
            update_queue: function (data) {
                this.currentQueue = data;
            },
            queue_ping: function() {
                this.$socket.emit("queue_pong")
            },
            update_timer:function(data) {
                this.currentTimer = data;
            },
            client_name: function(data) {
                this.clientName = data;
            }
        },
        data: function () {
            return {
                connected: false,
                currentQueue: [],
                ownId: "undefined",
                clientName: "undefined",
                currentTimer: 0,
            }
        },
        computed: {
            amIActive: function() {
                if(this.currentQueue.length == 0) return false;
                return this.currentQueue[1][0].id === this.ownId;
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

  .buttonLineNavi {
    margin: 2em;
  }
</style>