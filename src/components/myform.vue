<template>
  <div>
    <div v-for="memo in memos" :key="memo.id">
      <p @click="show(memo)">{{ memo.title }}</p>
    </div>
    <p v-if="memos.length < 1">保存されているメモはありません。</p>
    <p @click="add">+</p>
    <textarea v-model="memo" rows="15" cols="40"></textarea>
    <button @click="addMemo" v-if="status === 'new'">追加</button>
    <template v-else-if="status === 'edit'">
      <button @click="update">更新</button>
      <button @click="remove">削除</button>
    </template>
  </div>
</template>

<script>
export default {
  props: {
  },
  name: "myform",
  data() {
    return {
      memos: [],
      memo: '',
      id: 1,
      status: false,
      editId: null,
      ary1: []
    }
  },
  mounted() {
    if (localStorage.getItem('memos')) {
      this.memos = JSON.parse(localStorage.getItem('memos'));
      let ids = this.memos.map(memo => memo.id)
      this.id = ids.reduce((a, b) => Math.max(a, b)) + 1;
    }
  },
  methods: {
    addMemo() {
      if (!this.memo) {
        return;
      }
      let title = this.memo.split('\n')[0]
      this.memos.push({id: this.id, title: title, body: this.memo});
      this.memo = '';
      this.id++
      this.saveMemos();
    },
    saveMemos() {
      let parsed = JSON.stringify(this.memos);
      localStorage.setItem('memos', parsed);
    },
    show(memo) {
      this.memo = memo.body;
      this.editId = memo.id
      this.status = 'edit'
    },
    add() {
      this.status = 'new';
      this.memo = '';
    },
    update() {
      let title = this.memo.split('\n')[0];
      let index = this.memos.findIndex(memo => memo.id === this.editId);
      this.memos[index] = {id: index, title: title, body: this.memo};
      this.saveMemos();
    },
    remove() {
      let index = this.memos.map(memo => memo.id).indexOf(this.editId);
      this.memos.splice(index, 1);
      this.saveMemos();
      this.memo = '';
      this.status = false;
    }
  }
}
</script>

<style scoped>

</style>
