<template>
  <div>
    <div>Status: {{connectedMsg}}</div>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="go()">
      Servo 1
      <br>
      Up <b-icon icon="arrow-up-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="back()">
      Servo 1
      <br>
      Down <b-icon icon="arrow-down-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="turnLeft()">
      Servo 2
      <br>
      Up <b-icon icon="arrow-up-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="turnRight()">
      Servo 2
      <br>
      Down <b-icon icon="arrow-down-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="stop()">
      Stepper
      <br>
      Left <b-icon icon="arrow-left-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="stop()">
      Stepper
      <br>
      Right <b-icon icon="arrow-right-square"></b-icon>
    </b-button>
    <br>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="shutdown()">
      shutdown
    </b-button>
  </div>
</template>

<script>
export default {
  name: 'controls',
  data() {
    return {
      connectedMsg: 'Disconnected',
      connected: false,
    };
  },
  methods: {
    up() {
      this.$socket.emit('control-up', 10);
    },
    down() {
      this.$socket.emit('control-down', -10);
    },
    left() {
      this.$socket.emit('control-left', 10);
    },
    right() {
      this.$socket.emit('control-right', 100);
    },
    shutdown() {
      this.$socket.emit('system', 'shutdown');
    },
    keyDown(e) {
      if (e.code === 'ArrowUp') {
        this.go();
      } else if (e.code === 'KeyW') {
        this.go();
      } else if (e.code === 'ArrowLeft') {
        this.turnLeft();
      } else if (e.code === 'KeyA') {
        this.turnLeft();
      } else if (e.code === 'ArrowRight') {
        this.turnRight();
      } else if (e.code === 'KeyD') {
        this.turnRight();
      } else if (e.code === 'ArrowDown') {
        this.back();
      } else if (e.code === 'KeyS') {
        this.back();
      } else if (e.code === 'Space') {
        this.stop();
      }
      // do stuff
    },
  },
  sockets: {
    connect() {
      // eslint-disable-next-line no-console
      console.log('socket connected');
      this.connectedMsg = 'Connected';
      this.connected = true;
      this.$store.state.ownId = this.$socket.id;
      this.$socket.emit('register_front');
    },
    disconnect() {
      // eslint-disable-next-line no-console
      console.log('socket disconnected');
      this.connected = false;
      this.connectedMsg = 'Disonnected';
    },
    nsp_list: (data) => {
      // eslint-disable-next-line no-console
      console.log(`NSPs:${data}`);
    },
  },
  mounted() {
    this.sockets.subscribe('broadcastLike', (data) => {
      // eslint-disable-next-line no-console
      console.log(data);
    });
  },
  created() {
    window.addEventListener('keydown', this.keyDown);
  },
  destroyed() {
    window.removeEventListener('keydown', this.keyDown);
  },
};
</script>

<style>
  .buttons {
    margin: 10px;
  }
</style>