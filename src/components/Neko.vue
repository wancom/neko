<template>
  <div class="neko">
    <div v-if="status == 0" @mouseenter="$emit('mouseenter')">
      <img
        alt="catsit"
        src="../assets/sit.png"
        :style="{ left: x - 50 + 'px', top: y - 50 + 15 + 'px' }"
      />
    </div>

    <div v-if="status == 1" @mouseenter="$emit('mouseenter')">
      <img
        alt="catwalk2"
        src="../assets/walk3.png"
        :style="{
          left: x - 50 + 'px',
          top: y - 50 + 'px'
        }"
      />
    </div>
    <div v-if="status == 2">
      <img
        :alt="'catwalk' + String(walking)"
        :src="require('../assets/walk' + String(walking) + '.png')"
        :style="{
          left: x - 50 + 'px',
          top: y - 50 + 'px',
          transform:
            'rotate(' +
            getAngle() +
            'deg) rotateY(' +
            (targetx - x < 0 ? 0 : 180) +
            'deg)'
        }"
      />
    </div>
  </div>
</template>

<script>
// 0: sit
// 1: stand
// 2: walk
export default {
  name: "Neko",
  props: {
    ix: Number,
    iy: Number
  },
  data() {
    return {
      walking: 1,
      status: 0,
      x: this.ix | (window.innerWidth / 2),
      y: this.ix | (window.innerHeight / 2),
      targetx: this.ix | (window.innerWidth / 2),
      targety: this.ix | (window.innerHeight / 2),
      walktimer: null,
      sittimer: null,
      walkcount: 0
    };
  },
  mounted() {
    // document
    //   .getElementsByTagName("body")[0]
    //   .addEventListener("mousedown", e => {
    //     this.walkToTarget(e.clientX, e.clientY);
    //   });
  },
  methods: {
    walk(callback) {
      if (this.status == 2) {
        return;
      }
      clearInterval(this.sittimer);
      const self = this;
      this.status = 2;

      this.walktimer = setInterval(() => {
        if (
          Math.abs(self.targetx - self.x) < 3 &&
          Math.abs(self.targety - self.y) < 3
        ) {
          clearInterval(self.walktimer);
          self.status = 1;
          self.sittimer = setTimeout(() => {
            self.sit();
          }, 5000);
          if (callback) callback();
        }
        const dx = self.targetx - self.x;
        const dy = self.targety - self.y;
        const rad = Math.atan(dy / dx);
        const r = 3;
        if (dx != 0) self.x += Math.cos(rad) * r * (dx / Math.abs(dx));
        if (dy != 0)
          self.y += Math.sin(Math.abs(rad)) * r * (dy / Math.abs(dy));
        self.walkcount += 1;
        if (self.walkcount == 5) {
          self.walkcount = 0;
          self.walking += 1;
          if (self.walking == 9) {
            self.walking = 1;
          }
        }
      }, 50);
    },
    walkToTarget(x, y, callback) {
      this.targetx = x;
      this.targety = y;
      this.walk(callback);
    },
    sit() {
      if (this.status == 2) {
        clearInterval(this.walktimer);
      }
      this.status = 0;
    },
    getAngle() {
      const dx = this.targetx - this.x;
      const dy = this.targety - this.y;
      const rad = Math.atan(dy / dx);
      const deg = (rad * 180) / Math.PI;
      if (deg < 90) return deg;
      else return 180 - deg;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  width: 100px;
  height: 100px;
  position: absolute;
  /* top:320px;
  left:200px; */
}
</style>
