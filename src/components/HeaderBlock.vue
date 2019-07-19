<template>
  <header class="header">
    <h1><router-link :to="{ name: 'top' }">{{ ttl }}</router-link></h1>
    <nav class="nav">
      <ul class="navList">
        <slot></slot>
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
    align-items: center;
    border-bottom: 1px solid #ccc;
  }

  .logoutBtn {
    cursor: pointer;
    height: 40px;
    border-radius: 5px;
    margin-left: 1em;
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

