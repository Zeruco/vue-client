<template>
  <div id="app">
    <ClientForm />
  </div>
</template>

<script>
import ClientForm from "./components/ClientForm";

export default {
  name: 'App',
  components: {
    ClientForm
  }
}
</script>

<style lang="sass">
#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px

body
  background-color: #eee


*
  outline: none
  margin: 0
  padding: 0
  box-sizing: border-box

</style>
