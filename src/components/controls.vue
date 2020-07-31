<template>
  <div>
    <div>Status: {{connectedMsg}}</div>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="up1()">
      Servo 1
      <br>
      Up <b-icon icon="arrow-up-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="down1()">
      Servo 1
      <br>
      Down <b-icon icon="arrow-down-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="up2()">
      Servo 2
      <br>
      Up <b-icon icon="arrow-up-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="down2()">
      Servo 2
      <br>
      Down <b-icon icon="arrow-down-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="left()">
      Stepper
      <br>
      Left <b-icon icon="arrow-left-square"></b-icon>
    </b-button>
    <b-button class="buttons"
      :disabled="!connected || !$store.getters.amIActive" v-on:click="right()">
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
      this.$socket.emit('control-right', -10);
    },
    shutdown() {
      this.$socket.emit('system', 'shutdown');
    },
    keyDown(e) {
      if (e.code === 'ArrowUp') {
        this.up1();
      } else if (e.code === 'KeyW') {
        this.up2();
      } else if (e.code === 'KeyE') {
        this.down1();
      } else if (e.code === 'KeyS') {
        this.down2();
      } else if (e.code === 'KeyD') {
        this.left();
      } else if (e.code === 'KeyY') {
        this.right();
      } else if (e.code === 'KeyX') {
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
