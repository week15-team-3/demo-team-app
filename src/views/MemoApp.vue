<template>
  <div id="memo-app">
    <div class="postMemo__container">
      <h2>メモを追加しよう！</h2>
      <input
        type="text"
        placeholder="メモを入力してください"
        class="inputMemo"
        v-model="inputMemo"
      />
      <div>
        <h4>メモの背景色を選択してね！</h4>
        <div class="color-btn__flex">
          <div v-for="(color, index) in colors" v-bind:key="index">
            <input
              type="radio"
              v-bind:id="color.id"
              v-model="colorSelect"
              v-bind:value="color.bgc"
            /><label
              v-bind:for="color.id"
              class="radio-btn"
              v-bind:style="{ backgroundColor: color.bgc }"
            ></label>
          </div>
        </div>
      </div>

      <h4>プレビューが表示されるよ！</h4>
      <div
        class="preview-container"
        v-bind:style="{ backgroundColor: colorSelect }"
      >
        {{ inputMemo }}
      </div>
      <div style="color: red">
        {{ warning }}
      </div>
      <input
        class="addMemo-btn"
        type="submit"
        value="メモを追加する"
        v-on:click="addMemo"
      />
    </div>
    <h4>メモ一覧</h4>
    <div v-for="(memo, index) in reverseMemos" v-bind:key="index">
      <div v-bind:style="{ backgroundColor: memo.bgColor }" class="each__memo">
        <div>
          内容： {{ memo.content }} <br />
          日時： {{ memo.date }}
        </div>
        <div class="memo-btn">
          <div v-on:click="reMemo(index)">
            <img src="../assets/rememo.png" alt="メモ複製" />
          </div>
          <div v-on:click="deleteMemo(index)">
            <img src="../assets/delete.png" alt="削除" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      warning: "",
      inputMemo: "",
      memos: [],
      colorSelect: "",
      colors: [
        { id: "waterblue", bgc: "#dbfafa" },
        { id: "pink", bgc: "pink" },
        { id: "orange", bgc: "orange" },
        { id: "yellow", bgc: "yellow" },
        { id: "green", bgc: "rgb(126, 234, 130)" },
        { id: "silver", bgc: "silver" },
        { id: "aqua", bgc: "aqua" },
        { id: "purple", bgc: "#ffccff" },
        { id: "green2", bgc: "#86F9C5" },
        { id: "blue2", bgc: "#8EB8FF" },
      ],
    }
  },
  computed: {
    // 配列の要素順番を逆順にする
    reverseMemos() {
      return this.memos.slice().reverse()
    },
  },
  methods: {
    addMemo: function () {
      if (this.inputMemo != "" && this.colorSelect != "") {
        const memo = {
          content: this.inputMemo,
          date: new Date(),
          bgColor: this.colorSelect,
          reMemo: false,
        }
        this.memos.push(memo)
        this.inputMemo = ""
        this.colorSelect = ""
        this.warning = ""
      } else {
        this.warning = "*メモの内容と背景色を両方入力してください"
      }
    },
    deleteMemo: function (index) {
      this.memos.splice(index, 1)
    },
    reMemo: function (index) {
      const memo = {
        content: this.memos[index].content,
        date: new Date(),
        bgColor: this.memos[index].bgColor,
        reMemo: true,
      }
      this.memos.push(memo)
    },
  },
}
</script>

<style scoped>
.postMemo__container {
  background-color: rgb(159, 246, 217);
  border: thick double #32a1ce;
  border-radius: 25px;
  padding: 20px 50px;
  margin: 20px auto;
  width: 400px;
}

h2,
h4 {
  text-align: center;
}

input[type="text"] {
  font: 15px/24px sans-serif;
  box-sizing: border-box;
  width: 370px;
  padding: 0.3em;
  transition: 0.3s;
  letter-spacing: 1px;
  border: 1px solid #ffffff;
  box-shadow: 1px 1px 2px 0 #707070 inset;
  border-radius: 4px;
}

input[type="text"]:focus {
  outline: none;
  box-shadow: inset 1px 1px 2px 0 #c9c9c9;
}

input[type="radio"] {
  display: none;
}

.color-btn__flex {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.radio-btn {
  display: block;
  margin: 3px 9px;
  width: 57px;
  height: 30px;
  border-radius: 10px;
  cursor: pointer;
  transition: 0.3s; /* なめらか変化 */
  box-shadow: 6px 6px 4px #666666;
}

.radio-btn:hover {
  box-shadow: none;
}

.preview-container {
  display: inline-block;
  border: 5px ridge rgba(211, 220, 50, 0.6);
  width: 370px;
  min-height: 50px;
  text-align: center;
  line-height: 50px;
}

.addMemo-btn {
  display: inline-block;
  border-radius: 20px;
  border: none;
  text-align: center;
  cursor: pointer;
  padding: 12px 12px;
  margin: 10px 30%;
  background: #4dffff;
  color: #990000;
  transition: 0.3s; /* なめらか変化 */
  box-shadow: 6px 6px 4px #666666;
}
.addMemo-btn:hover {
  box-shadow: none;
  color: #4dffff;
  background: #990000;
}

/* ここからはメモ表示に関するCSS */
.each__memo {
  display: block;
  margin: 5px 170px 5px 100px;
  padding: 30px 50px;
  border-radius: 30px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.memo-btn {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.memo-btn img {
  display: inline-block;
  width: 40px;
  cursor: pointer;
}
</style>
