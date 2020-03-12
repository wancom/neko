<template>
  <div class="nekojarashi-field" @mousemove="njmove" @touchmove="njmove">
    <div
      class="nekojarashi-cover"
      @mousedown="grabChanged(!isGrabbing)"
      @touchstart.prevent="grabChanged(true)"
      @touchend="grabChanged(false)"
      ref="nekojarashi"
      :style="{
        left: this.NJx - 25 + 'px',
        top: this.NJy - 25 + 'px',
        transform: isShaking ? 'rotate(5deg)' : 'rotate(-5deg)'
      }"
    >
      <img
        class="nekojarashi-img"
        alt="nekojarashi"
        src="../assets/items/nekojarashi.png"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isGrabbing: false,
      NJx: window.innerWidth - 50,
      NJy: window.innerHeight - 50,
      shakeTimer: null,
      isShaking: false
    };
  },
  mounted() {
    this.shake();
    this.$emit("nekojarashi", [this.NJx, this.NJy]);
  },
  methods: {
    grabChanged(isGrab) {
      this.isGrabbing = isGrab;
      this.$emit("grab", this.isGrabbing);
      if (this.isGrabbing) {
        this.$emit("njmove", [this.NJx, this.NJy]);
      }
      this.shake();
    },
    njmove(e) {
      if (this.isGrabbing) {
        this.NJx = e.pageX;
        this.NJy = e.pageY;
        this.$emit("njmove", [this.NJx, this.NJy]);
      }
    },
    shake() {
      clearTimeout(this.shakeTimer);
      this.isShaking = !this.isShaking;
      if (this.isShaking && !this.isGrabbing) {
        this.shakeTimer = setTimeout(this.shake, 2000);
      } else {
        this.shakeTimer = setTimeout(this.shake, 250);
      }
    }
  }
};
</script>

<style>
.nekojarashi-field {
  width: 100vw;
  height: 100vh;
  z-index: 999;
}

.nekojarashi-cover {
  position: absolute;
  display: flex;
  width: 50px;
  height: 50px;
  top: 10px;
  right: 10px;
  justify-content: center;
  align-items: center;
  user-select: none;
  animation-duration: 100ms;
}
.nekojarashi-img {
  width: 50px;
  height: 50px;
  z-index: -1;
  user-select: none;
  -webkit-touch-callout: none;
}
</style>
