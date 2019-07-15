<template>
  <div id="app" class="container">
    <header class = "header">TODO LIST by</header>
    <img src="./assets/logo.png" class="logo">
    <TodoList/>
    <br>
    <br>
    <button class="calc" @click = "Calculator()"> Calculator </button>
    <Calculator />
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue';
import TodoList from './components/TodoList.vue';
import DatePicker from 'vue2-datepicker';

export default {
  name: 'App',
  components: {
    Calculator,
    TodoList,
    DatePicker
  },
  methods:{ 
    Calculator(){
      
    }
  }
}
</script>

<style>

* {
  box-sizing: border-boc;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}

.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
}

.header{
  text-align: center;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.calc{
  display: block;
  margin: 20px auto;
  width: 100px;
}
</style>
