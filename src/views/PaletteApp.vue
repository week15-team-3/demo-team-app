<template>
  <h1>Vue パレット</h1>
  <div class="app">
    <div
      class="palette"
      v-bind:style="{ 'background-color': rgba(0, 100, 200, 0.5) }"
      v-on:mousemove="changeColor"
    ></div>
    <p>rgba( {{ red }}, {{ green }}, 200, 0.5 )</p>
    <div class="colors-container">
      <div class="mini-palette"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      red: 0,
      green: 0,
      blue: 0,
      alpha: 0,
    }
  },
  methods: {
    changeColor(event) {
      this.red = event.offsetX
      this.green = event.offsetY
    },
  },
}
</script>

<style>
.app {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
}
.palette {
  width: 255px;
  height: 255px;
}
.mini-palette {
  min-width: 60px;
  height: 60px;
}
.colors-container {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
  padding-top: 8px;
}
</style>
