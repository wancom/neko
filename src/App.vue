<template>
  <div id="app">
    <Neko
      v-for="i in 5"
      :key="i"
      :ref="'neko' + i"
      @mouseenter="
        () => {
          randomWalk(i, setRandomWalk);
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
      randtimer: [null]
    };
  },
  mounted() {
    for (var i = 1; i <= Object.keys(this.$refs).length; i++) {
      this.randtimer.push(null);
      this.$refs["neko" + i][0].x =
        Math.floor(Math.random() * (window.innerWidth - 100)) + 50;
      this.$refs["neko" + i][0].y =
        Math.floor(Math.random() * (window.innerHeight - 100)) + 50;
      this.setRandomWalk(i);
    }
  },
  methods: {
    randomWalk(i, callback) {
      clearTimeout(this.randtimer[i]);
      const x = Math.floor(Math.random() * window.innerWidth);
      const y = Math.floor(Math.random() * window.innerHeight);
      this.$refs["neko" + i][0].walkToTarget(x, y, () => {
        callback(i);
      });
    },
    setRandomWalk(i) {
      clearTimeout(this.randtimer[i]);
      const self = this;
      var ms = Math.floor(Math.random() * 50) * 100;
      if (ms == 0) {
        ms = 60 * 1000;
      }
      this.randtimer[i] = setTimeout(() => {
        self.randomWalk(i, self.setRandomWalk);
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
