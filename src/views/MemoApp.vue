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
        <input
          type="radio"
          id="white"
          v-model="colorSelect"
          value="white"
        /><label
          for="white"
          class="radio-btn"
          style="background-color: white"
        ></label>
        <input
          type="radio"
          id="pink"
          v-model="colorSelect"
          value="pink"
        /><label
          for="pink"
          class="radio-btn"
          style="background-color: pink"
        ></label>
        <input
          type="radio"
          id="orange"
          v-model="colorSelect"
          value="orange"
        /><label
          for="orange"
          class="radio-btn"
          style="background-color: orange"
        ></label>
        <input
          type="radio"
          id="yellow"
          v-model="colorSelect"
          value="yellow"
        /><label
          for="yellow"
          class="radio-btn"
          style="background-color: yellow"
        ></label>
        <input
          type="radio"
          id="green"
          v-model="colorSelect"
          value="rgb(126, 234, 130)"
        /><label
          for="green"
          class="radio-btn"
          style="background-color: rgb(126, 234, 130)"
        ></label>
        <br />
        <input
          type="radio"
          id="silver"
          v-model="colorSelect"
          value="silver"
        /><label
          for="silver"
          class="radio-btn"
          style="background-color: silver"
        ></label>
        <input
          type="radio"
          id="aqua"
          v-model="colorSelect"
          value="aqua"
        /><label
          for="aqua"
          class="radio-btn"
          style="background-color: aqua"
        ></label>
        <input
          type="radio"
          id="purple"
          v-model="colorSelect"
          value="#FFCCFF"
        /><label
          for="purple"
          class="radio-btn"
          style="background-color: #ffccff"
        ></label>
        <input
          type="radio"
          id="green2"
          v-model="colorSelect"
          value="#86F9C5"
        /><label
          for="green2"
          class="radio-btn"
          style="background-color: #86f9c5"
        ></label>
        <input
          type="radio"
          id="blue2"
          v-model="colorSelect"
          value="#8EB8FF"
        /><label
          for="blue2"
          class="radio-btn"
          style="background-color: #8eb8ff"
        ></label>
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

    <div v-for="(memo, index) in memos" v-bind:key="index">
      <div v-bind:style="{ backgroundColor: memo.bgColor }">
        内容： {{ memo.content }} <br />
        日時： {{ memo.date }}
        <button v-on:click="deleteMemo(index)">削除</button>
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
    }
  },
  methods: {
    addMemo: function () {
      if (this.inputMemo != "" && this.colorSelect != "") {
        const memo = {
          content: this.inputMemo,
          date: new Date(),
          bgColor: this.colorSelect,
        }
        this.memos.push(memo)
        this.inputMemo = ""
        this.colorSelect = ""
        this.warning = ""
      } else {
        this.warning = "*メモの内容と背景色を入力してください"
      }
    },
    deleteMemo: function (index) {
      this.memos.splice(index, 1)
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

.radio-btn {
  display: inline-block;
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
</style>
