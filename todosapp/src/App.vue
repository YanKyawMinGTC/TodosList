<template>
  <toDos />
</template>

<script>
import toDos from './components/todos.vue'

export default {
  name: 'App',
  components: {
    toDos
  },
  directives: {
  focus: {
    // directive definition
    inserted: function (el) {
        console.log(el)
      el.focus()
    }
  }
}
}
</script>

<style>
* {
  box-sizing: border-box;
}
.container {
    max-width: 600px;
    margin: 0 auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 14px;
}
</style>
