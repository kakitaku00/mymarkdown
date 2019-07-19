<template>
  <header class="header">
    <h1><router-link :to="{ name: 'top' }">{{ ttl }}</router-link></h1>
    <nav class="nav">
      <ul class="navList">
        <li class="navListItem">ABOUT</li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  name: "headerBrock",
  props: ["ttl"],
  data() {
    return {

    }
  },
}
</script>

<style lang="scss" scoped>
  .header {
    width: 100%;
    padding: 25px 50px;
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #ccc;
  }
</style>

