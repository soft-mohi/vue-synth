<template>
  <div>
    <h3>VCF</h3>
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
      q:
      <input type="range" min="0" max="50" id="" value="5" v-model="Q" />
    </div>
  </div>
</template>

<script>
export default {
  name: "VCF",
  props: {
    ctx: null
  },
  data: function() {
    return {
      filter: null,
      type: "lowpass",
      freq: 440,
      Q: 5
    };
  },

  watch: {
    freq() {
      this.filter.frequency.value = this.freq;
    },
    Q() {
      this.filter.Q.value = this.Q;
    }
  },

  methods: {
    setup() {
      this.filter = new BiquadFilterNode(this.ctx, {
        frequency: this.freq,
        q: this.Q
      });
      this.filter.type = this.type;
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
