<template>
  <div class="neko">
    <div v-if="status == 0" @mouseenter="$emit('mouseenter')">
      <img alt="catsit" :src="basePath + 'sit.png'" :style="stylePosition" />
    </div>

    <div v-if="status == 1" @mouseenter="$emit('mouseenter')">
      <img
        alt="catwalk3"
        :src="basePath + 'walk3.png'"
        :style="stylePosition"
      />
    </div>
    <div v-if="status == 2">
      <img
        :alt="'cat' + walkPicName"
        :src="basePath + walkPicName + '.png'"
        :style="
          Object.assign(stylePosition, {
            transform:
              'rotate(' +
              (Math.atan((targety - y) / (targetx - x)) * 180) / Math.PI +
              'deg) rotateY(' +
              (targetx - x < 0 ? 0 : 180) +
              'deg)'
          })
        "
      />
    </div>
  </div>
</template>

<script>
const STATUS_SIT = 0;
const STATUS_STAND = 1;
const STATUS_WALK = 2;
const NUM_OF_PICT = 8;
export default {
  name: "Neko",
  props: {
    ix: String,
    iy: String
  },
  data() {
    return {
      x: parseInt(this.ix) || window.innerWidth / 2,
      y: parseInt(this.iy) || window.innerHeight / 2,
      status: STATUS_SIT,
      walkspeed: 3,
      catassets: "silhouette",
      walkcount: 0,
      targetx: parseInt(this.ix) | (window.innerWidth / 2),
      targety: parseInt(this.iy) | (window.innerHeight / 2),
      walktimer: null,
      sittimer: null
    };
  },
  computed: {
    basePath() {
      return "./img/cat/" + this.catassets + "/";
    },
    walkPicName() {
      return "walk" + String(Math.floor(this.walkcount / this.walkspeed) + 1);
    },
    stylePosition() {
      return { left: this.x - 50 + "px", top: this.y - 50 + "px" };
    }
  },
  methods: {
    walk(callback) {
      if (this.status == STATUS_WALK) {
        clearInterval(this.walktimer);
      }
      clearInterval(this.sittimer);
      const self = this;
      this.status = STATUS_WALK;

      this.walktimer = setInterval(() => {
        const dx = self.targetx - self.x;
        const dy = self.targety - self.y;

        if (dx ** 2 + dy ** 2 < this.walkspeed) {
          // Reach terget position
          clearInterval(self.walktimer);
          self.status = STATUS_STAND;
          self.sittimer = setTimeout(() => {
            self.sit();
          }, 5000);
          if (callback) callback();
        }

        const rad = Math.abs(Math.atan(dy / dx));
        if (dx != 0)
          self.x += Math.cos(rad) * (dx < 0 ? -1 : 1) * this.walkspeed;
        if (dy != 0)
          self.y += Math.sin(rad) * (dy < 0 ? -1 : 1) * this.walkspeed;
        if (self.walkcount + 1 < this.walkspeed * NUM_OF_PICT)
          self.walkcount += 1;
        else self.walkcount = 0;
      }, 50);
    },
    walkToTarget(x, y, callback) {
      this.targetx = x;
      this.targety = y;
      this.walk(callback);
    },
    sit() {
      if (this.status == STATUS_WALK) {
        clearInterval(this.walktimer);
      }
      this.status = STATUS_SIT;
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
}
</style>
