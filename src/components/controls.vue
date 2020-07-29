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
    up1() {
      this.$socket.emit('control-up1', 10);
    },
    up2() {
      this.$socket.emit('control-up2', 10);
    },
    down1() {
      this.$socket.emit('control-down1', -10);
    },
    down2() {
      this.$socket.emit('control-down2', -10);
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
        this.up1();
      } else if (e.code === 'ArrowUp1') {
        this.up2();
      } else if (e.code === 'ArrowUp2') {
        this.down1();
      } else if (e.code === 'ArrowDown1') {
        this.down2();
      } else if (e.code === 'ArrowDown2') {
        this.left();
      } else if (e.code === 'ArrowLeft') {
        this.right();
      } else if (e.code === 'ArrowRight') {
      // do stuff
    }
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
},
}
</script>

<style>
  .buttons {
    margin: 10px;
  }
</style>
