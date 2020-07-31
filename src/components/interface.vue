<template>
  <div>
    <div>
      <div id="controlscontainer">
        <strong>Connected: {{connected}}</strong><br>
        <div class="half">
          <!-- <button class="buttonGo" v-on:click="go()" !$store.getters.amIActive>GO</button> -->
          <button class="buttonGo" v-on:click="go()" :disabled="!connected || !$store.getters.amIActive">GO</button>
          <button class="buttonStop" v-on:click="stop()" :disabled="!connected || !$store.getters.amIActive">STOP</button><br>
            <hr/>
            <!-- Override Key: <input type="text" v-model="overridePW" v-on:change="authorize"> -->
        </div>
        <!-- <div class="half">
          <h4>Current Waiting Queue {{this.currentTimer}}</h4>
          <SimpleQueue :currentQueue="currentQueue" :ownId="ownId" :ownName="clientName"></SimpleQueue>
        </div> -->
      </div>
      <b-button class="info" v-b-toggle.sidebar-1>Informationen</b-button>
      <b-sidebar id="sidebar-1" title="Informationen" shadow>
        <div class="px-3 py-2">
          <b-img class="image" src="../assets/foto.jpg" fluid thumbnail></b-img>
          <h2>Line Follower</h2>
          <p>
            Unser ferngesteuertes Kameraauto ist mit Infrarotsensoren ausgestattet, die zwischen weiß und schwarz
            unterscheiden können. So kann dem Line Follower eine bestimmte Route durch Linien vorgegeben werden.
          </p>
          <br />
          <h5>Aufbau</h5>
          <p>
            Für unser Projekt benötigten wir neben vier Rädern mit Gleichstrommotoren auch zwei Plexiglasplatten, die
            unsere Installation tragen.
            Für die Installation und die Umsetzung der Technik verwendeten wir einen Arduino für die Steuerung und eine
            H-Brücke für die Verbindung zu den Motoren.
            Um alles auf einem Interface steuerbar zu machen, benötigten wir einen Raspberry Pi, der zudem mit einer
            Kamera ausgestattet ist,
            der die übermittelten Daten vom Backend des Frontends abholt und an den Arduino übermittelt.
          </p>
          <b-img class="image" src="../assets/arduinoWires.jpg" fluid></b-img>
          <br />
          <br />
          <b-img class="image" src="../assets/sensor.jpeg" fluid></b-img>
          <br />
          <br />
          <h5>Entwicklung</h5>
          <p>
            Bevor wir das Auto zusammengebaut haben, schauten wir zuerst nach dem Antrieb der Motoren. Anschließend
            verlöteten und verbauten wir die Motoren an die Räder und bauten alle Autoteile zusammen.
            Jetzt entwickelten wir Code, um das Auto ohne Sensor anzusteuern. Danach versuchten wir zunächst die
            Sensordaten des Infrarotsensors auszulesen. Hier änderten wir den Code mithilfe einer while-Schleife, sodass
            wir nur noch 1 oder 0 als Wert bekommen haben. So ist 0 der Wert für weiß und 1 für schwarz.
            Auf dieser Grundlage erstellten wir den Code, der Sensor und Auto zusammenbrachten.
            Zunächst fuhr der Line Follower nur auf der Geraden und konnte keine Kurven fahren, bis wir die Räder in
            jedem einzelnen Fall des Abbiegens verschieden ansteuerten. So dreht bei einer Links-Kurve das rechte Rad
            schneller und bei einer Rechts-Kurve das linke.
          </p>
          <br />
          <video width="320" height="240" controls>
            <source src="../assets/step1.mp4" type="video/mp4">
          </video>
          <br/>
          <video width="320" height="240" controls>
            <source src="../assets/carDriving.mp4" type="video/mp4">
          </video>
          <br/>
          <b-img class="image" src="../assets/prototyping.jpg" fluid></b-img>
          <br />
        </div>
      </b-sidebar>
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
      },
      // authorize: function() {
      //   this.$socket.emit('authorize', this.overridePW);
      // }
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
      // authorized: function(data) {
      //   this.authorized = data;
      // }

    },
    data: function () {
      return {
        connected: false,
        overridePW: "",
        // authorized: false
      }
    },

  }
</script>

<style scoped>
  .buttonGo {
    margin: 3vw;
    margin-bottom: 2vw;
    /* padding: 10 em; */
    border-radius: 9%;
    width: 8vw;
    height: 5vh;
  }

  .buttonGo:hover {
    background-color: #7F9E38;
    color: whitesmoke;
  }

  .buttonStop {
    margin: 3vw;
    margin-bottom: 2vw;
    /* padding: 10 em; */
    border-radius: 9%;
    width: 8vw;
    height: 5vh;
  }

  .buttonStop:hover {
    background-color: #E88072;
    color: whitesmoke;
  }

  p {
    display: flex;
    text-align: left;
  }

  .headline {
    font-weight: bold;
  }

  .image {
    border-radius: 15px;
  }

  .camerastream {
    width: 500 vw;
    height: 2000 vh;
    border-style: solid;
  }

  strong {
    margin: 1vw;
  }

  .info {
    background-color: #0F404F;
    color: whitesmoke;
    border-style: none;
    padding: 0.5em;
    font-weight: bold;
  }

  .info:hover {
    background-color: #4C899C;
    color: whitesmoke;
    border-style: none;
    padding: 0.5em;
  }
</style>