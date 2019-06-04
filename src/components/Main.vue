<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,
      <br />check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener"
        >vue-cli documentation</a
      >.
    </p>
    <div>
      <div>
        <input type="radio" id="sine" value="sine" v-model="type" />
        <label for="sine">sine</label>
        <input type="radio" id="square" value="square" v-model="type" />
        <label for="square">square</label>
        <input type="radio" id="sawtooth" value="sawtooth" v-model="type" />
        <label for="sawtooth">sawtooth</label>
        <input type="radio" id="triangle" value="triangle" v-model="type" />
        <label for="triangle">triangle</label>
      </div>
      <div>
        freq:
        <input
          type="range"
          min="50"
          max="3000"
          id="freq"
          value="440"
          v-model="freq"
        />
      </div>
      <div>
        gain:
        <input
          type="range"
          min="0"
          max="1"
          step="0.01"
          value="0.5"
          id="level"
          v-model="gain"
        />
      </div>

      <button @click="start">start</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  props: {
    msg: String
  },
  data: function() {
    return {
      ctx: null,
      osc: null,
      type: "sawtooth",
      gain: 0.5,
      gainNode: null,
      freq: 440,

      isPlaying: false
    };
  },

  mounted() {
    this.ctx = new AudioContext();
  },

  watch: {
    type() {
      if (this.isPlaying) {
        this.osc.type = this.type;
      }
    },
    freq() {
      if (this.isPlaying) {
        this.osc.frequency.value = this.freq;
      }
    },
    gain() {
      if (this.isPlaying) {
        this.gainNode.gain.value = this.gain;
      }
    }
  },

  methods: {
    start() {
      if (this.isPlaying) {
        this.isPlaying = false;
        this.osc.stop();
        return;
      }
      this.isPlaying = true;
      this.gainNode = new GainNode(this.ctx);
      this.osc = new OscillatorNode(this.ctx);
      this.osc.type = this.type;
      this.osc.frequency.value = this.freq;
      this.gainNode.gain.value = this.gain;
      this.osc.connect(this.gainNode).connect(this.ctx.destination);
      this.osc.start();
    }
  }
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
