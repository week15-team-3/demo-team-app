<template>
  <h1>Vue パレット</h1>
  <div class="app">
    <!-- 使い方の説明 -->
    <h2 class="explanation_title">使い方</h2>
    <p class="explanation_main">
      マウスを動かすと縦軸、メインパレットの横軸に設定されたRGBAの要素の値が変化します。<br />メインパレット上でクリックすると一時的に色の変化を止められます。再びクリックすると色の変化が再開します。
    </p>
    <!-- 説明終わり -->
    <!-- 以下からメインパレットの大枠 -->
    <h3 class="palette_title">メインパレット</h3>
    <div class="palette_area">
      <!-- 下からY軸を表す部分 -->
      <div class="palette_Yaxis palette_axis">
        <div class="palette_area_axis_index">Low</div>
        <div class="palette_area_axis_element">
          <select v-model="selectedX" class="palette_area_axis_element">
            <option
              v-for="(color, index) in colorElementsList"
              v-bind:value="color"
              v-bind:key="index"
            >
              {{ color.text }}
            </option>
          </select>
        </div>
        <div class="palette_area_axis_index">High</div>
      </div>
      <!-- Y軸を表す部分終わり -->
      <!-- 下のdivタグが色を表示 -->
      <div
        class="palette"
        v-bind:style="{
          'background-color':
            'rgba(' + red + ', ' + green + ', ' + blue + ', ' + alpha + ')',
        }"
        v-on:mousemove="changeColor"
        v-on:click="decideColor"
      ></div>
      <!-- 色を表示する部分ここまで -->
      <!-- 下からX軸を表す部分 -->
      <div class="palette_axis palette_Xaxis">
        <div class="palette_area_axis_index">Low</div>
        <dic class="palette_area_axis_element"
          ><select v-model="selectedY" class="palette_area_axis_element">
            <option
              v-for="(color, index) in colorElementsList"
              v-bind:value="color"
              v-bind:key="index"
            >
              {{ color.text }}
            </option>
          </select></dic
        >
        <div class="palette_area_axis_index">High</div>
      </div>
      <!-- X軸を表す部分終わり -->
    </div>
    <!-- 下のpタグが現在の色の説明 -->
    <p>rgba( {{ red }}, {{ green }}, {{ blue }}, {{ alpha }} )</p>
    <!-- 現在の色の説明終わり -->
    <div class="colors-container">
      <div class="mini-palette"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      decideTrigger: false,
      Xpointer: 0,
      Ypointer: 0,
      red: 0,
      green: 0,
      blue: 200,
      alpha: 0.5,
      colorElementsList: [
        { text: "Red", value: [this.red] },
        { text: "Green", value: [this.green] },
        { text: "Blue", value: [this.blue] },
        { text: "Alpha", value: [this.alpha] },
      ],
      selectedX: { text: "Green", value: [this.green] },
      selectedY: { text: "Red", value: [this.red] },
    }
  },
  methods: {
    // 以下はマウスを動かしたときに色を変更する関数。クリックされるたびに色の変化が起こるか止まるかが変化する機構を内包している。
    changeColor(event) {
      this.Xpointer = event.offsetX
      this.Ypointer = event.offsetY
      if (!this.decideTrigger) {
        if (this.selectedX.text === "Red") {
          this.red = this.Xpointer
        }
        if (this.selectedX.text === "Green") {
          this.green = this.Xpointer
        }
        if (this.selectedX.text === "Blue") {
          this.blue = this.Xpointer
        }
        if (this.selectedX.text === "Alpha") {
          this.alpha = this.Xpointer
        }
        if (this.selectedY.text === "Red") {
          this.red = this.Ypointer
        }
        if (this.selectedY.text === "Green") {
          this.green = this.Ypointer
        }
        if (this.selectedY.text === "Blue") {
          this.blue = this.Ypointer
        }
        if (this.selectedY.text === "Alpha") {
          this.alpha = this.Ypointer
        }
      }
    },
    // 以下はクリックのたびに変数名"decideColor"のboolean値を変える関数
    decideColor() {
      if (this.decideTrigger) {
        this.decideTrigger = false
      } else {
        this.decideTrigger = true
      }
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
  width: 425px;
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
