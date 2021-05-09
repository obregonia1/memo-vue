<template>
  <div class="memobody">
    <div class="memolist">
      <div v-for="memo in memos" :key="memo.id">
        <p @click="show(memo)" class="memotitle">{{ memo.title }}</p>
      </div>
      <p v-if="memos.length < 1">メモはありません。</p>
      <p @click="add">+</p>
    </div>
    <div class="form_with_button">
      <textarea v-model="memo" rows="12" cols="30" v-if="status"></textarea>
      <br>
      <button @click="addMemo" v-if="status === 'new'">追加</button>
      <template v-else-if="status === 'edit'">
        <button @click="update">更新</button>
        <button @click="remove">削除</button>
      </template>
    </div>
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
    }
  },
  mounted() {
    if (localStorage.getItem('memos')) {
      this.memos = JSON.parse(localStorage.getItem('memos'));
    }
  },
  methods: {
    addMemo() {
      if (!this.memo) {
        return;
      }
      let title = this.memo.split('\n')[0]
      if (this.memos[0]) {
        let ids = this.memos.map(memo => memo.id)
        this.id = ids.reduce((a, b) => Math.max(a, b)) + 1;
      }
      this.memos.push({id: this.id, title: title, body: this.memo});
      this.memo = '';
      this.id++;
      this.saveMemos();
    },
    saveMemos() {
      let parsed = JSON.stringify(this.memos);
      localStorage.setItem('memos', parsed);
    },
    show(memo) {
      this.memo = memo.body;
      this.editId = memo.id
      this.status = 'edit';
    },
    add() {
      this.status = 'new';
      this.memo = '';
    },
    getIndex() {
      return this.memos.findIndex(memo => memo.id === this.editId);
    },
    update() {
      let title = this.memo.split('\n')[0];
      let index = this.getIndex();
      let body = this.memo
      this.memos[index] = {id: index, title: title, body: body};
      this.saveMemos();
      this.memo = '';
      this.memo = body;
    },
    remove() {
      let index = this.getIndex();
      this.memos.splice(index, 1);
      this.saveMemos();
      this.memo = '';
      this.status = false;
    }
  }
}
</script>

<style scoped>
template {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.memobody {
  display: flex;
  justify-content: center;
}
.memolist {
  width: 200px;
}
.memolist p {
  font-size: 20px;
}
.form_with_button {
  width: 400px;
}
textarea {
  font-size: 18px;
}
</style>
