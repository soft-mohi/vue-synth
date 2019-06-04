<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <!-- <Main msg="Welcome to Vue synthesizer" /> -->

    <VCO :ctx="ctx" :freq="note" ref="VCO1" />
    <VCF :ctx="ctx" ref="VCF1" />
    <VCA :ctx="ctx" ref="VCA1" />

    <button @click="start">start</button>
    <button @click="stop">stop</button>

    <div class="keyboard">
      <ul class="key-list">
        <li
          v-for="(note, i) in notes"
          :key="i"
          class="key"
          :class="{ '-sharp': isSharp(i) }"
          @mousedown="stroke(note)"
          @mouseup="release"
          @mouseleave="release"
        ></li>
      </ul>
    </div>
  </div>
</template>

<script>
import Main from "./components/Main.vue";
import VCO from "./components/VCO.vue";
import VCF from "./components/VCF.vue";
import VCA from "./components/VCA.vue";

export default {
  name: "app",
  components: {
    Main,
    VCO,
    VCF,
    VCA
  },
  data: function() {
    return {
      ctx: null,
      isPlaying: false,
      note: 440
    };
  },

  computed: {
    notes() {
      return [...Array(88)].map(
        (_, i) => 440 * Math.pow(2, (1 / 12) * (-48 + i))
      );
    }
  },

  mounted() {
    this.ctx = new AudioContext();
  },

  methods: {
    start() {
      if (this.isPlaying) {
        return;
      }
      this.$refs.VCA1.setup();
      this.$refs.VCA1.amplifier.connect(this.ctx.destination);
      this.$refs.VCF1.setup();
      this.$refs.VCF1.filter.connect(this.$refs.VCA1.amplifier);
      this.$refs.VCO1.setup();
      this.$refs.VCO1.osc.connect(this.$refs.VCF1.filter);
      this.$refs.VCO1.start();
      this.isPlaying = true;
    },

    stop() {
      if (!this.isPlaying) {
        return;
      }
      this.isPlaying = false;
      this.$refs.VCO1.stop();
    },
    isSharp(i) {
      return [1, 4, 6, 9, 11].includes(i % 12);
    },
    stroke(note) {
      this.note = note;
    },
    release() {
      return;
    }
  }
};
</script>

<style lang="scss" scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.keyboard {
  padding: 10px 0;
  background-color: rgba(6, 6, 6, 0.3);
}

.key-list {
  height: 250px;
  background-color: #666666;

  /* 改行させないため */
  overflow-x: scroll;
  overflow-y: hidden;
  white-space: nowrap;

  > .key {
    cursor: pointer;
    display: inline-block;

    width: 60px;
    height: calc(100% - 6px); /* scrollbar分 */
    background-color: #ffffff;
    margin: 0 3px;

    border-radius: 0 0 5px 5px;

    &.-sharp {
      position: relative;
      width: 0;
      margin: 0;

      &::after {
        display: block;
        content: "";

        position: absolute;
        top: 0;
        left: calc(-50px / 2);
        width: 50px;
        height: 50%;

        border-radius: 0 0 5px 5px;
        background-color: #000000;
      }
    }
  }
}
</style>
