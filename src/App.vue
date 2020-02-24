<template>
  <div id="app">
    <Neko
      ref="neko"
      @mouseenter="
        () => {
          randomWalk(setRandomWalk);
        }
      "
    />
  </div>
</template>

<script>
import Neko from "./components/Neko.vue";

export default {
  name: "App",
  components: {
    Neko
  },
  data() {
    return {
      randtimer: null
    };
  },
  mounted() {
    this.setRandomWalk();
  },
  methods: {
    randomWalk(callback) {
      clearTimeout(this.randtimer);

      const x = Math.floor(Math.random() * window.innerWidth);
      const y = Math.floor(Math.random() * window.innerHeight);
      this.$refs.neko.walkToTarget(x, y, callback);
    },
    setRandomWalk() {
      clearTimeout(this.randtimer);
      const self = this;
      const ms = Math.floor(Math.random() * 5) * 1000;
      console.log(ms);
      if (ms == 0) return;
      this.randtimer = setTimeout(() => {
        this.randomWalk(self.setRandomWalk);
      }, ms);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
body {
  overflow: hidden;
  width: 100%;
  height: 100%;
}
html {
  width: 100%;
  height: 100%;
}
</style>
