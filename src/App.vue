<template>
  <div id="app">
    <myheader />
    <myform />
  </div>
</template>

<script>
import myform from "@/components/myform";
import myheader from "@/components/myheader";

export default {
  components: {
    myform,
    myheader
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
