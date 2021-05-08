<template>
  <div id="app">
    <textarea v-model="memo" rows="15" cols="40"></textarea>
    <button @click="addMemo">Add memo</button>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      memos: [],
      memo: '',
      id: 0
    }
  },
  mounted() {
    if (localStorage.getItem('memos')) {
      this.memos = JSON.parse(localStorage.getItem('memos'));
      let ids = this.memos.map(memo => memo.id)
      this.id = ids.reduce((a, b) => Math.max(a, b));
    }
  },
  methods: {
    addMemo() {
      if (!this.memo) {
        return;
      }
      this.memos.push({id: this.id + 1, body: this.memo});
      this.memo = '';
      this.id++
      this.saveMemos();
    },
    saveMemos() {
      let parsed = JSON.stringify(this.memos);
      localStorage.setItem('memos', parsed);
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
