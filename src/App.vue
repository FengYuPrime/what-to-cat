<template>
  <div id="app">
    <Order />
  </div>
</template>

<script>
import Order from "./components/Order"
export default {
  name: 'App',
  components: {
    Order,
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  list-style: none;
}
</style>
