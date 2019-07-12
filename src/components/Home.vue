<template>
<div class="home">
  <Header :title="msg"></Header>
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
import Header from "./Header"

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
    Header: Header
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
    margin-top: 30px;
  }
</style>
