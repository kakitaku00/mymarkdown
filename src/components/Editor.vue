<template>
  <div class="editor">
    <Header-Block :ttl="msg">
      <li class="userStatus">
        <span>{{ user.displayName }}</span>
        <button class="logoutBtn" @click="logout">ログアウト</button>
      </li>
    </Header-Block>
    <div>
      <div class="memoListWrapper">
        <div class =" memoList" v-for ="(memo, index) in memos" :key="index" @click ="selectMemo(index)" :data-selected="index == selectedIndex">
          <p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
        </div>
        <button class="btn addMemoBtn" @click="addMemo">メモの追加</button>
        <button class="btn deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo"> 選択中のメモの削除</button>
        <button class="btn saveMemoBtn" @click="saveMemos">メモの保存</button>
      </div>
    </div>
    <div class="editorWrapper">
      <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
      <div class="preview markdown-body" v-html="preview()"></div>
    </div>
  </div>
</template>

<script>
import HeaderBlock from "./HeaderBlock"

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
    HeaderBlock: HeaderBlock
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

  .editorWrapper {
    display: flex;
  }

  .markdown {
    float: left;
    width: 50%;
    height: 500px;
  }

  .preview {
    float: left;
    width: 50%;
    text-align-last: left;
    padding: 20px;
    border: 1px solid #ccc;
  }

  .btn {
    cursor: pointer;
    height: 40px;
    width: 90%;
    border-radius: 5px;
    white-space: nowrap;
    box-shadow: 1px 1px 0px 1px rgba(0,0,0,0.05);
    border: 1px solid #ccc;
    transition-property: background-color, box-shadow;
    transition-duration: 150ms;
    transition-timing-function: ease-in-out;

    &:not(:first-of-type) {
      margin-top: 10px;
    }

    &:focus,
    &:hover {
      box-shadow: 1px 4px 5px 1px rgba(0,0,0,0.1);
    }

    &:active {
      background-color: #e5e5e5;
      box-shadow: none;
      transition-duration: 10ms;
    }
  }
</style>


