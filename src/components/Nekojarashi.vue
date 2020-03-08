<template>
  <div class="nekojarashi-field" @mousemove="nekojarashi">
    <div
      class="nekojarashi-cover"
      @mousedown="isGrabbing = !isGrabbing"
      ref="nekojarashi"
    >
      <img
        class="nekojarashi-img"
        alt="nekojarashi"
        src="../assets/nekojarashi.png"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isGrabbing: false,
      NJx: window.innerWidth - 10,
      NJy: window.innerHeight - 10,
      toId: null,
      isShaking: false
    };
  },
  mounted() {
    this.shake();
  },
  methods: {
    nekojarashi(e) {
      if (this.isGrabbing) {
        this.NJx = e.x - 20;
        this.NJy = e.y - 20;
        this.$refs["nekojarashi"].style.top = this.NJy + "px";
        this.$refs["nekojarashi"].style.left = this.NJx + "px";
        this.$emit("nekojarashi", [this.NJx, this.NJy]);
      }
      this.$emit("grab", this.isGrabbing);
    },
    shake() {
      clearTimeout(this.toId);
      if (this.isShaking) {
        this.$refs["nekojarashi"].style.transform = "rotate(5deg)";
        this.toId = setTimeout(this.shake, 250);
      } else {
        this.$refs["nekojarashi"].style.transform = "rotate(-5deg)";
        this.toId = setTimeout(this.shake, 2000);
      }
      this.isShaking = !this.isShaking;
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
}
</style>
