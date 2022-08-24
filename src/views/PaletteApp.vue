<template>
  <h1>Vue パレット</h1>
  <div class="app">
    <!-- 使い方の説明 -->
    <h2 class="explanation_title">使い方</h2>
    <p class="explanation_main">
      マウスを動かすと縦軸、メインパレットの横軸に設定されたRGBAの要素の値が変化します。<br />メインパレット上または「色の変化」の部分でクリックすると一時的に色の変化を止められます。再びクリックすると色の変化が再開します。
    </p>
    <!-- 説明終わり -->
    <!-- 以下からメインパレットの大枠 -->
    <h3 class="palette_title title">メインパレット</h3>
    <!-- クリックの状態を表示。下のdivタグ内の関数decideColorの引数はパレット外でクリックされることを表します。 -->
    <div class="clickStatus" v-on:click="decideColor(false)">
      色の変化：
      <div v-if="decideTrigger">停止中</div>
      <div v-else>可能</div>
    </div>
    <!-- クリックの状態表示終わり -->
    <div class="palette_area">
      <!-- 下からY軸を表す部分 -->
      <div class="palette_Yaxis palette_axis">
        <div class="palette_area_axis_index">0</div>
        <div class="palette_area_axis_element">
          <select v-model="selectedY" class="palette_area_axis_element">
            <option
              v-for="(color, index) in colorElementsList"
              v-bind:value="color"
              v-bind:key="index"
            >
              {{ color.text }}
            </option>
          </select>
        </div>
        <div class="palette_area_axis_index">255</div>
      </div>
      <!-- Y軸を表す部分終わり -->
      <!-- 下のdivタグが色を表示。divタグ内にある関数decideColorの引数はpalette上でクリックしていることを表します。 -->
      <div
        class="palette"
        v-bind:style="{
          'background-color':
            'rgba(' + red + ', ' + green + ', ' + blue + ', ' + alpha + ')',
        }"
        v-on:mousemove="changeColor"
        v-on:click="decideColor(true)"
      >
        <!-- 下のdivタグはクリックしたところをさすポインター -->
        <div
          v-show="decideTrigger && isFocusPalette"
          class="palette_pointer"
          v-bind:style="{
            left: pointerPositionX - 5 + 'px',
            top: pointerPositionY - 5 + 'px',
          }"
        ></div>
        <!-- ポインター終わり -->
      </div>
      <!-- 色を表示する部分ここまで -->
      <!-- 下からX軸を表す部分 -->
      <div class="palette_axis palette_Xaxis">
        <div class="palette_area_axis_index">0</div>
        <dic class="palette_area_axis_element"
          ><select v-model="selectedX" class="palette_area_axis_element">
            <option
              v-for="(color, index) in colorElementsList"
              v-bind:value="color"
              v-bind:key="index"
            >
              {{ color.text }}
            </option>
          </select></dic
        >
        <div class="palette_area_axis_index">255</div>
      </div>
      <!-- X軸を表す部分終わり -->
    </div>
    <!-- 下のpタグが現在の色のRGBA値を表す -->
    <p v-show="!decideTrigger">
      rgba(
      {{ red }}, {{ green }}, {{ blue }}, {{ alpha }} )
    </p>

    <p v-show="decideTrigger">
      rgba(
      <input
        class="inputColor"
        v-model="red"
        v-bind:placeholder="red"
        v-on:click="focusInput"
        type="number"
      />
      ,
      <input
        class="inputColor"
        v-model="green"
        v-bind:placeholder="green"
        v-on:click="focusInput"
        type="number"
      />,
      <input
        class="inputColor"
        v-model="blue"
        v-bind:placeholder="green"
        v-on:click="focusInput"
        type="number"
      />,
      <input
        class="inputColor"
        v-model="alpha"
        v-bind:placeholder="alpha"
        v-on:click="focusInput"
        type="number"
      />
      )
    </p>
    <p class="colorCode" v-show="!decideTrigger">
      カラーコード:{{ colorCode }} <br />透過度:{{ alpha }}
    </p>
    <p class="colorCode" v-show="decideTrigger">
      カラーコード:<input
        class="colorCodce_input"
        v-model="inputColorCode"
        v-bind:placeholder="colorCode"
        v-on:click="focusInput"
      />
      <br />
      透過度:<input
        class="inputColor"
        v-model="alpha"
        v-bind:placeholder="alpha"
        v-on:click="focusInput"
        type="number"
      />
    </p>
    <!-- 現在の色を表す部分終わり -->
    <!-- 保存ボタン -->
    <button class="saveButton" v-on:click="saveColor">保存</button>
    <!-- 保存ボタン終わり -->
    <!-- 以下から保存した色を表示する部分 -->
    <h3 class="colors-container_title title">保存した色</h3>
    <div class="colors-container">
      <div v-if="savedColors.length === 0" class="displayNothing">Nothing</div>
      <div
        v-else
        v-for="(savedColor, index) in savedColors"
        v-bind:key="index"
        class="mini-palette_area"
      >
        <div
          class="mini-palette"
          v-bind:style="{
            'background-color':
              'rgba(' +
              savedColor.red +
              ',' +
              savedColor.green +
              ',' +
              savedColor.blue +
              ',' +
              savedColor.alpha +
              ')',
          }"
          v-on:click="displaySavedColor(index)"
          tabindex="0"
        ></div>
        <button class="deleteColorButton" v-on:click="deleteColor(index)">
          削除
        </button>
      </div>
    </div>
    <!-- 保存した色を表示する部分終わり -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      decideTrigger: false,
      Xpointer: 0,
      Ypointer: 0,
      pointerPositionX: this.Xpointer,
      pointerPositionY: this.Ypointer,
      red: 0,
      green: 0,
      blue: 200,
      alpha: 0.5,
      colorElementsList: [
        { text: "None" },
        { text: "Red" },
        { text: "Green" },
        { text: "Blue" },
        { text: "Alpha" },
      ],
      selectedX: { text: "Green" },
      selectedY: { text: "Red" },
      savedColors: [],
      isFocusPalette: true,
      inputColorCode: "",
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
          this.alpha = this.Xpointer / 255
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
          this.alpha = this.Ypointer / 255
        }
      }
    },
    // 以下はクリックのたびに変数名"decideColor"のboolean値を変える関数
    decideColor(clickInPalette) {
      this.isFocusPalette = true
      if (this.decideTrigger) {
        this.decideTrigger = false
      } else {
        if (clickInPalette) {
          this.pointerPositionX = this.Xpointer
          this.pointerPositionY = this.Ypointer
        }
        this.decideTrigger = true
      }
    },
    saveColor() {
      const theColor = { red: "", green: "", blue: "", alpha: "" }
      theColor.red = this.red
      theColor.green = this.green
      theColor.blue = this.blue
      theColor.alpha = this.alpha
      this.savedColors.push(theColor)
    },
    deleteColor(index) {
      this.savedColors.splice(index, 1)
    },
    focusInput() {
      this.isFocusPalette = false
    },
    displaySavedColor(index) {
      this.red = this.savedColors[index].red
      this.green = this.savedColors[index].green
      this.blue = this.savedColors[index].blue
      this.alpha = this.savedColors[index].alpha
      this.isFocusPalette = false
    },
  },
  computed: {
    colorCode() {
      let colorCode =
        "#" +
        ("00" + this.red.toString(16)).slice(-2) +
        ("00" + this.green.toString(16)).slice(-2) +
        ("00" + this.blue.toString(16)).slice(-2)
      return colorCode
    },
  },
  watch: {
    inputColorCode() {
      this.red = parseInt(this.inputColorCode.substr(1, 2), 16)
      this.green = parseInt(this.inputColorCode.substr(3, 2), 16)
      this.blue = parseInt(this.inputColorCode.substr(5, 2), 16)
    },
  },
}
</script>

<style scoped>
p {
  margin: 5px 0px;
}
.app {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
}
.clickStatus {
  display: flex;
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
.inputColor {
  width: 2.5rem;
}
.colorCode {
  text-align: center;
}
.colorCodce_input {
  width: 4.5rem;
}
.mini-palette {
  min-width: 60px;
  height: 60px;
  border-radius: 30px;
}
.mini-palette:focus {
  border: solid 2px;
}
.palette_pointer {
  background-color: white;
  position: relative;
  width: 10px;
  height: 10px;
  border: solid 2px;
  border-radius: 10px;
}
.deleteColorButton {
  width: 60px;
}
.colors-container_title {
  margin-top: 30px;
}
.displayNothing {
  margin-left: auto;
  margin-right: auto;
}
.colors-container {
  display: flex;
  flex-wrap: wrap;
  width: 360px;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-right: 10px;
  border: 5px solid;
}
.mini-palette_area {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
  margin-left: 10px;
}
</style>
