<template>
  <h1>Vue パレット</h1>
  <div class="app">
    <div class="palette_area">
      <div class="palette_Yaxis palette_axis">
        <div class="palette_area_axis_index">Low</div>
        <div class="palette_area_axis_element">Green</div>
        <div class="palette_area_axis_index">High</div>
      </div>
      <!-- 下のdivタグが色を表示 -->
      <div
        class="palette"
        v-bind:style="{
          'background-color':
            'rgba(' + red + ', ' + green + ', ' + blue + ', ' + alpha + ')',
        }"
        v-on:mousemove="changeColor"
      ></div>
      <!-- 色を表示する部分ここまで -->
      <div class="palette_axis palette_Xaxis">
        <div class="palette_area_axis_index">Low</div>
        <dic class="palette_area_axis_element">Red</dic>
        <div class="palette_area_axis_index">High</div>
      </div>
    </div>
    <!-- 下のpタグが現在の色の説明 -->
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
      blue: 200,
      alpha: 0.5,
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
.palette_area {
  width: 375px;
  display: flex;
  flex-wrap: wrap;
}
.palette_axis {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.palette_Yaxis {
  flex-direction: column;
}
.palette_Xaxis {
  width: 255px;
  margin-left: auto;
  margin-right: auto;
}
.palette_area_axis_element {
  font-size: 20px;
  font-weight: bold;
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
