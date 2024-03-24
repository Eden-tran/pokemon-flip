<template>
  <div class="screen">
    <h1>Pokemon memories</h1>
    <h3>Select mode to start</h3>
    <div class="action">
      <button @click="onStart(16)">
        <span>4X4</span>
        <span>EASY</span>
      </button>
      <button @click="onStart(36)">
        <span>6X6</span>
        <span>NORMAL</span>
      </button>
      <button @click="onStart(64)">
        <span>8X8</span>
        <span>HARD</span>
      </button>
      <button @click="onStart(100)">
        <span>10X10</span>
        <span>EASY</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    onStart(totalOfBlocks) {
      this.$emit("onStart", { totalOfBlocks });
    },
  },
};
</script>

<style>
.div {
}
</style>
