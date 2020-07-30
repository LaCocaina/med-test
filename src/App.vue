<template>
  <div id="app">
    <Form />
  </div>
</template>

<script>

import Form from "@/components/Form";
export default {
  name: 'App',
  components: {
    Form
  }
}
</script>

<style>
  * {
    margin: 0;
    outline: none;
  }

#app {
  margin: 16px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
</style>
