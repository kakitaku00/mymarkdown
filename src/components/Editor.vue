<template>
  <div class="editor">
    <Header :title="msg"></Header>
    <span>{{ user.displayName }}</span>
    <button @click="logout">ログアウト</button>
    <div>
      <div class="memoListWrapper">
        <div class =" memoList" v-for ="(memo, index) in memos" :key="index" @click ="selectMemo(index)" :data-selected="index == selectedIndex">
          <p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
        </div>
        <button class="addMemoBtn" @click="addMemo">メモの追加</button>
        <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo"> 選択中のメモの削除</button>
        <button class="saveMemoBtn" @click="saveMemos">メモの保存</button>
      </div>
    </div>
    <div class="editorWrapper">
      <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
      <div class="preview markdown-body" v-html="preview()"></div>
    </div>
  </div>
</template>

<script>
import Header from "./Header"

import marked from "marked";
export default {
  name: "editor",
  props: ["user"],
  data() {
    return {
      memos: [
        {
          markdown: ""
        }
      ],
      selectedIndex: 0,
      msg: "Editor"
    };
  },
  created: function() {
    firebase
      .database()
      .ref('memos/' + this.user.uid)
      // 1回だけ読み込む
      .once('value')
      .then(result => {
        // データを取得
        if (result.val()) {
          this.memos = result.val()
        }
      })
  },
  methods: {
    logout: function() {
      firebase.auth().signOut();
    },
    addMemo: function() {
      this.memos.push({
        markdown: "無題のメモ"
      })
    },
    deleteMemo: function() {
      this.memos.splice(this.selectedIndex, 1);
      if (this.selectedIndex > 0) {
        this.selectedIndex--;
      }
    },
    saveMemos: function() {
      firebase
        // DBに接続
        .database()
        // データを読むパスを指定
        .ref('memos/' + this.user.uid)
        // 保存
        .set(this.memos);
    },
    selectMemo: function(index) {
      this.selectedIndex = index;
    },
    preview: function() {
      return marked(this.memos[this.selectedIndex].markdown)
    },
    displayTitle: function (text) {
      return text.split(/\n/)[0]
    }
  },
  mounted: function() {
    document.onkeydonw = e => {
      if (e.key == 's' && e.metaKey) {
        this.saveMemos();
        return false;
      }
    }
  },
  beforeDestroy: function() {
    document.onkeydown = null;
  },
  components: {
    Header: Header
  }
}
</script>

<style lang="scss" scoped>
.memoListWrapper {
  width: 19%;
  float: left;
  border-top: 1px solid #000;
}

.memoList {
  padding: 10px;
  box-sizing: border-box;
  text-align-last: left;
  border-bottom: 1px solid #000;

  &:nth-child(even) {
    background-color: #ccc;
  }

  &[data-selected="true"] {
    background-color: #ccf;
  }
}

.memoTitle {
  height: 1.5em;
  margin: 0;
  word-spacing: nowrap;
  overflow: hidden;
}

.addMemoBtn {
  margin-top: 20px;
}

.deleteMemoBtn {
  margin: 10px;
}

.editorWrapper {
  display: flex;
}

.markdown {
  float: left;
  width: 40%;
  height: 500px;
}

.preview {
  float: left;
  width: 40%;
  text-align-last: left;
}
</style>


