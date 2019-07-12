<template>
  <header class="header">
    <h1><router-link :to="{ name: 'top' }">{{ title }}</router-link></h1>
    <nav class="nav">
      <ul class="navList">
        <li class="navListItem">ABOUT</li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  name: "header",
  props: ["title"],
  data() {
    return {
      title: title
    }
  }
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

