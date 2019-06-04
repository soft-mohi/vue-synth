<template>
  <div>
    <h3>VCO</h3>
    <div>
      <input type="radio" id="sawtooth" value="sawtooth" v-model="type" />
      <label for="sawtooth">sawtooth</label>
      <input type="radio" id="square" value="square" v-model="type" />
      <label for="square">square</label>
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
  </div>
</template>

<script>
export default {
  name: "VCO",
  props: {
    ctx: AudioContext,
    freq: {
      type: Number,
      default: 440
    }
  },
  data: function() {
    return {
      osc: null,
      type: "sawtooth",
      isPlaying: false
    };
  },

  mounted() {},

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
    }
  },

  methods: {
    setup() {
      this.osc = new OscillatorNode(this.ctx);
      this.osc.type = this.type;
      this.osc.frequency.value = this.freq;
    },

    start() {
      this.osc.start();
      this.isPlaying = true;
    },

    stop() {
      this.osc.stop();
      this.isPlaying = false;
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
