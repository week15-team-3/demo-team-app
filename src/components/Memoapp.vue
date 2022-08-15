<template>
  <div id="memo-app">
    <input type="text" maxlength="25" v-model="memoContent" /> <br />
    <select size="3" v-model="colorSelect">
      <option selected="selected">背景色を選ぶ</option>
      <option value="pink">ピンク</option>
      <option value="orange">オレンジ</option>
      <option value="blue">ブルー</option>
      <option value="green">グリーン</option>
    </select>
    <span v-bind:style="{ backgroundColor: colorSelect }">
      ({ memoContent })
    </span>
    <button v-on:click="addMemo" v-bind:disabled="typing">メモを追加</button>

    <div v-for="(memo, index) in memos" v-bind:key="index">
      <div v-bind:style="{ backgroundColor: memo.bgColor }">
        内容： ({ memo.content }) <br />
        日時： ({ memo.date })
        <button v-on:click="deleteMemo(index)">削除</button>
      </div>
    </div>
  </div>
</template>

<script>
const memoApp = {
  data() {
    return {
      memoContent: "",
      memos: [],
      colorSelect: "",
    }
  },
  methods: {
    addMemo: function () {
      const memo = {
        content: this.memoContent,
        date: new Date(),
        bgColor: this.colorSelect,
      }
      this.memos.push(memo)
    },
    deleteMemo: function (index) {
      this.memos.splice(index, 1)
    },
    typing: function () {
      if (this.memoContent === "") {
        return false
      } else {
        return true
      }
    },
  },
}
Vue.createApp(memoApp).mount("#memo-app")
</script>
