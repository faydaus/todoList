<template>
  <div>
      <input type="text" class="todo-input" placeholder="Add List ...." v-model="newTodo" @keydown.enter="addTodo()">
      <date-picker class= "date" v-model="datetime" lang="en" type="datetime" format="[on] YYYY-MM-dd [at] HH:mm:ss" placeholder="Due Date" confirm></date-picker>
      <br>
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated bounceOutRight"> 
        <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
            
            <div class = "todo-item-left">
                <input type="checkbox" v-model="todo.completed">
                <div v-if="!todo.editing" @dblclick="editTodo(todo)" class = "todo-item-label" :class="{completed : todo.completed}">  <!--how to set two classes when one class has condition -->
                    {{todo.title}}
                </div>  
                <input v-else class = "todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)"
                @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus placeholder="edit mode">
            </div>
                <div class="remove-item" @click="removeTodo(index)">
                    &times;
                </div>  
        </div>
    </transition-group>
      <div class="extra-container">

          <div>
              <label>
                  <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodo()"> 
                  Check All
              </label>
          </div>

          <div>
            <button :class="{active: filter == 'all'}" @click="filter = 'all'">All({{all}})</button>
            <button :class="{active: filter == 'active'}" @click="filter = 'active'">Active({{active}})</button>
            <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed({{completed}})</button>
          </div>

          <div>
              <transition name="fade">
                <button v-if="showClearCompletedButton" @click="clearCompleted()">Clear Completed</button>
              </transition>
          </div>

          <div>
              {{remaining}} items left
          </div>

      </div>

  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker'

export default {
  name: 'todo-list',
  components:{DatePicker},
  data () {
    return {
      newTodo: '',
      idForTodo: 1,
      beforeEditCache: '',
      filter: 'all',
      todos: [
          {
            'id':'1',
            'title': 'start adding to-do here',
            'completed': false,
            'editing': false
          }
       ],
       datetime: ''
    }
  },
//computed property is for composing new data drive from other data
//should not muted any of other data
//should not accept parameter
//always return something
  computed:{      
      remaining(){
          return this.todos.filter(todo => !todo.completed).length;
      },

      anyRemaining(){
          return this.remaining != 0;
      },

      todosFiltered(){
          if (this.filter == 'all') {
              return this.todos;
          }
          else if(this.filter == 'active'){
              return this.todos.filter(todo => !todo.completed);
          }
          else if(this.filter=='completed'){
              return this.todos.filter(todo => todo.completed);
          }
          return this.todos
      },

      showClearCompletedButton(){
          return this.todos.filter(todo => todo.completed).length > 0;
      },

      all(){
          return this.todos.length;
      },

      active(){
          return this.todos.filter(todo => !todo.completed).length;
      },

      completed(){
          return this.todos.filter(todo => todo.completed).length;
      }
  }, 

  directives:{
      focus:{
          //directive defination
          inserted: function(el){
              el.focus();
          }
      }
  },

  methods: {
      addTodo(){
          if (this.newTodo.trim().length == 0){
              return;

          }
          alert ('New List is added');
          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false,
              date: Date()
          })

          this.newTodo = '';
          thia.idForTodo ++;
      },

      removeTodo(index){
          this.todos.splice(index,1);
      },

      editTodo(todo){
          this.beforeEditCache = todo.title;
          todo.editing = true;
      },

      doneEdit(todo){
          if(todo.title.trim() == ''){
              alert('You have inserted empty list (back to default list)');
              todo.title = this.beforeEditCache;
          }
          todo.editing = false;
      },

      cancelEdit(todo){
          todo.title = this.beforeEditCache;
          todo.editing = false;
      },

      checkAllTodo(){
          this.todos.forEach((todo) => todo.completed = event.target.checked)
      },

      clearCompleted(){
          this.todos = this.todos.filter(todo => !todo.completed);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css);
@import url(https://raw.githubusercontent.com/daneden/animate.css/master/animate.css);

 .todo-input{
     width: 100%;
     padding: 10px 18px;
     font-size: 18px;
     margin-bottom: 16px;

     &:focus{
         outline: 0;
     }
 }

  .todo-item{
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
  }

  .remove-item{
      cursor: pointer;
      margin-left: 14px;
      &:hover{
          color:black;
      }
  }

  .todo-item-left{
      display: flex;
      align-items: center;
  }

  .todo-item-label{
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      grid-auto-rows: minmax(50px, auto); 
      padding: 10px;
      border: 1px solid purple;
      margin-left: 12px;
  }

  .todo-item-edit{
      font-size: 24px;
      color: #2c3e50;
      margin-left: 12px;
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      font-family: Arial, Helvetica, sans-serif;

      &:focus{
          outline: none;
      }
  }

  .completed{
      text-decoration: line-through;
      color: red;
  }

  .extra-container{
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 16px;
      border-top: 1px solid lightgrey;
      padding-top: 14px;
      margin-bottom: 14px;
  }

  button{
      font-size: 14px;
      background: white;
      appearance: none;

  &:hover{
      background: lightgreen;
  }

  &:focus{
      outline: none;
  }
}

  .active{
      background: lightgreen;
  }

  .fade-enter-active{
      transition: opacity .5s;
  }

  .fade-enter, .fade-leave-to{
      opacity: 0;
  }

  .bounce-out-active{
      transition: opacity .5s;
  }
  .date{
      display: block;
  }
</style>
