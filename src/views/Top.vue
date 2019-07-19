<template>
<div id="top">
  <div class="loading" v-if="!loading">Loading...</div>
  <Home v-if="!isLogin && loading"></Home>
  <Editor v-if="isLogin && loading" :user="userData"></Editor>
  <div v-if="isLogin && loading">
    <router-link :to="{ name: 'terms' }">利用規約</router-link>
  </div>
</div>
</template>

<script>
import Home from "../components/Home.vue";
import Editor from "../components/Editor.vue";
export default {
  name: "top",
  data() {
    return {
      loading: false,
      isLogin: false,
      userData: null
    };
  },
  beforeCreate: function() {
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
      if (user) {
        this.isLogin = true;
        this.userData = user;
      } else {
        this.isLogin = false;
        this.userData = null;
      }
      // 最低1秒loading表示
      setTimeout(() => {
        this.loading = true;
      }, 1000);
    })
  },
  components: {
    Home: Home,
    Editor: Editor
  }
};
</script>

<style lang="scss">
#top {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.loading {
  margin-top: 100px;
}
</style>