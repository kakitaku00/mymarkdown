<template>
<div class="home">
  <HeaderBlock :titleMsg="msg"></HeaderBlock>
  <div class="container">
    <section class="section">
      <header>
        <h2 class="section__title">SIMPLE Markdown Editor</h2>
      </header>
      <p class="section__text">最小限の機能に絞ったシンプルなエディターです</p>
    </section>
    <button class="loginBtn" @click="googleLogin">Googleアカウントでログイン</button>
  </div>
</div>
</template>

<script>
import HeaderBlock from "./HeaderBlock"

export default {
  name: 'home',
  data() {
    return {
      msg: 'Welcome to MyMarkdown'
    }
  },
  methods: {
    googleLogin: function() {
      firebase.auth().signInWithRedirect(new firebase.auth.GoogleAuthProvider());
    }
  },
  components: {
    HeaderBlock: HeaderBlock
  }
}
</script>

<style lang="scss" scoped>
  .section {
    &__title {
      font-size: 24px;
    }

    &__text {
      margin-top: 20px;
    }
  }

  .loginBtn {
    cursor: pointer;
    margin-top: 30px;
    height: 40px;
    border-radius: 5px;
    white-space: nowrap;
    box-shadow: 1px 1px 0px 1px rgba(0,0,0,0.05);
    border: 1px solid #ccc;
    transition-property: background-color, box-shadow;
    transition-duration: 150ms;
    transition-timing-function: ease-in-out;

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
