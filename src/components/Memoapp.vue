<template>
  <div id="memo-app">
    <input type="text" maxlength="25" v-model="inputMemo" /> <br />
    <select size="5" v-model="colorSelect">
      <option selected="selected">背景色を選ぶ</option>
      <option value="pink">ピンク</option>
      <option value="orange">オレンジ</option>
      <option value="yellow">イエロー</option>
      <option value="green">グリーン</option>
    </select>
    <span v-bind:style="{ backgroundColor: colorSelect }">{{ inputMemo }}</span>

    <input
      class="modal-button"
      type="submit"
      value="メモを追加"
      v-on:click="addMemo"
    />

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
      inputMemo: "",
      memos: [],
      colorSelect: "",
    }
  },
  methods: {
    addMemo: function () {
      const memo = {
        content: this.inputMemo,
        date: new Date(),
        bgColor: this.colorSelect,
      }
      this.memos.push(memo)
      this.inputMemo = ""
    },
    deleteMemo: function (index) {
      this.memos.splice(index, 1)
    },
  },
}
</script>
