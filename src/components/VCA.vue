<template>
  <div>
    <h3>VCA</h3>
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
  </div>
</template>

<script>
export default {
  name: "VCA",
  props: {
    ctx: null
  },
  data: function() {
    return {
      amplifier: null,
      gain: 0.5
    };
  },

  mounted() {},

  watch: {
    gain() {
      this.amplifier.gain.value = this.gain;
    }
  },

  methods: {
    setup() {
      this.amplifier = new GainNode(this.ctx);
      this.amplifier.gain.value = this.gain;
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
