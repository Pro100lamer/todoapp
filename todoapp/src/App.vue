<template>
  <div
    id="app"
    class="conteiner"
  >
    <ToDoList />
  </div>
</template>

<script>
import ToDoList from './components/ToDoList'

export default {
  name: 'App',
  components: {
    ToDoList
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
body{
  background: #f2f2f2;
}

.conteiner{
  max-width: 900px;
  margin: 20px auto;
}

</style>