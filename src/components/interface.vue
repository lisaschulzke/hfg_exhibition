<template>

  <div>
    <b-button v-b-toggle.sidebar-1> <span class="glyphicon glyphicon-menu-burger" aria-hidden="true">Informationen</span>
    </b-button>
    <b-sidebar id="sidebar-1" title="Informationen" shadow>
      <div class="px-3 py-2">
        <b-img class="image"  src="../assets/foto.jpg" fluid thumbnail></b-img>
        <h2>Line Follower</h2>
        <p>
          Unser ferngesteuertes Kameraauto, ist mit Infrarotsensoren ausgestattet, die zwischen weiß und schwarz unterscheiden können. So kann dem Line Follower eine bestimmte Route durch Linien vorgegeben werden.
          </p>
          <br/>
          <h5>Aufbau</h5>
          <p>
            Für unser Projekt benötigten wir neben vier Rädern mit Gleichstrommotoren auch zwei Plexiglasplatten, die unsere Installation tragen. 
            Für die Installation und die Umsetzung der Technik verwendeten wir einen Arduino für die Steuerung und eine H-Brücke für die Verbindung zu den Motoren.
            Um alles auf einem Interface steuerbar zu machen, benötigten wir einen Raspberry Pi, der zudem mit einer Kamera ausgestattet ist, 
            der die übermittelten Daten vom Backend des Frontends abholt und an den Arduino übermittelt.
          </p>

          <br/>
          <h5>Entwicklung</h5>
          <p>
            Bevor wir das Auto zusammengebaut haben, schauten wir zuerst nach dem Antrieb der Motoren. Anschließend verlöteten und verbauten wir die Motoren an die Räder und bauten alle Autoteile zusammen.
            Jetzt entwickelten wir Code, um das Auto ohne Sensor anzusteuern. Danach versuchten wir zunächst die Sensordaten des Infrarotsensors auszulesen. Hier änderten wir den Code mithilfe einer while-Schleife, sodass wir nur noch 1 oder 0 als Wert bekommen haben. So ist 0 der Wert für weiß und 1 für schwarz. 
            Auf dieser Grundlage erstellten wir den Code, der Sensor und Auto zusammen brachten.
          </p>
        <br/>
      </div>
    </b-sidebar>
    <div>
      <div id="controlscontainer">
        <strong>Connected: {{connected}}</strong><br><br><br>
        <div class="half">
          <button v-on:click="go()" :disabled="!connected || !$store.getters.amIActive">GO</button>
          <button v-on:click="stop()" :disabled="!connected || !$store.getters.amIActive">STOP</button><br>
          <!-- <b-button class="buttonLineNavi" v-on:click = "go ('send message', $event)" >start</b-button> -->
          <!-- <b-button class="buttonLineNavi" v-on:click = "stop ('send message', $event)">stop</b-button> -->
        </div>
        <!-- <div class="half">
          <h4>Current Waiting Queue {{this.currentTimer}}</h4>
          <SimpleQueue :currentQueue="currentQueue" :ownId="ownId" :ownName="clientName"></SimpleQueue>
        </div> -->
      </div>
    </div>
  </div>


</template>

<script>
  // import SimpleQueue from "./SimpleQueueComponent";

    export default {
        components: {
            // SimpleQueue
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
            
        },
        data: function () {
            return {
                connected: false,
                
            }
        },
        
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

  p{
    display: flex;
    text-align: left;
  }

  .headline{
    font-weight: bold;
  }

  .image{
    border-radius: 15px;
  }

  .camerastream {
    width: 500 vw;
    height: 2000 vh;
    border-style: solid;
  }
</style>