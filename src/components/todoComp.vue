<template>
  <div class="container-box">
    <div class="todo-list-title">TodoList</div>
    <div class="input-bar">
      <input type="text" class="todo-input" placeholder="What needs to be done...?"
    v-model="newTodo"
    @keyup.enter="addTodo"
    @blur="clearInput">
    </div>

    <transition-group name="fade" enter-active-class="animate__animated animate__fadeInUp" 
    leave-active-class="animate__animated animate__fadeOutDown">
    
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" :class="{ completed : todo.completed}"  class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" id="checkbox" v-model="todo.completed">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)"
        class="todo-item-label"> {{todo.title}} </div>
        <input v-else class="todo-item-edit" type="text" v-focus 
        @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)"
        @keyup.esc="cancelEdit(todo)"
        v-model="todo.title">
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        <i class="far fa-trash-alt"></i>
      </div>
    </div>

    </transition-group>

    <div class="bottom-container">
      <div class="check-all" v-if="todos.length > 0">
        <input type="checkbox" @click="checkAll" :checked="!anyRemaining">
        Check All
      </div>
      <div class="items-left">
      {{remaining}} items left.
      </div>
    </div>
    <div class="bottom-container">
      <div>
        <button :class="{ active : filter == 'all' }" @click="filter = 'all'">All ({{this.todos.length}})</button>
        <button :class="{ active : filter == 'active' }" @click="filter = 'active'">Active  ({{ this.todos.filter(todo => !todo.completed).length }})</button>
        <button :class="{ active : filter == 'completed' }" @click="filter = 'completed'">Completed ({{ this.todos.filter(todo => todo.completed).length }})</button>
      </div>
      <div class="clear-completed">
        <transition name="fade">
          <button class="clear-completed-button" 
          v-if="completedCheck"
          @click="clearCompleted">Clear Completed</button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data (){
    return{
      newTodo: '',
      idForTodo: 0,
      titleBeforeEdit: '',
      filter: 'all',
      todos: []
    }
  },
  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining(){
      return this.remaining != 0;
    },
    todosFiltered(){
      if(this.filter == 'all'){
        return this.todos
      }
      else if (this.filter == 'active'){
        return this.todos.filter(todo => !todo.completed)
      }
      else if (this.filter == 'completed'){
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    completedCheck(){
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  directives: {
  focus: {
    inserted: function (el) {
      el.focus()
    }
  }
},
  methods: {
    addTodo(){
      if (this.newTodo.trim().length == 0){
        return
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false
        
      })
      this.newTodo ='',
      this.idForTodo++
    },
    removeTodo(index){
      this.todos.splice(index, 1);
    },
    editTodo(todo){
      this.titleBeforeEdit = todo.title;
      todo.editing = true;
    },
    doneEdit(todo){
      if (todo.title.trim().length == 0){
        todo.title = this.titleBeforeEdit;
      }
      todo.editing = false;
    },
    cancelEdit(todo){
      todo.title = this.titleBeforeEdit;
      todo.editing = false;
    },
    checkAll(){
      this.todos.forEach((todo) => todo.completed =
      event.target.checked);
    },
    clearInput(){
      this.newTodo = ''
    },
    clearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}

</script>

<style lang="scss">

@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css");


.container-box{
  font-weight: 100;
  background-color: #fff;
  padding: 30px;
  width: 500px;
}
.todo-input{
  font-size: 1.3rem;
  width: 100%;
  padding: 11px;
  border: none;
  color: #534D41;


  &:focus{
    outline: none;
    border: 1px solid #534D41;
    padding: 10px;
  }
  &::placeholder{
    color: #cacaca;
  }
}
.todo-item{
  font-size: 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 10px 0;
  animation-duration: 0.6s;
  transition: all 0.2s ease-in-out;
}

.remove-item{
  display: none;
  margin-right: 10px;
  cursor: pointer;
  &:hover{
    color: #534D41;
  }
}
.todo-item:hover{
  background-color:#F9F9F9;
}
.todo-item:hover  .remove-item{
  display: block;
}
.todo-item-left{
  display: flex;
  align-items: center;

}
.todo-item-label{
  font-size: 1.5rem;
  padding: 13px;
  margin-left: 1rem;
  word-wrap: break-word; 
  width: 360px;

}
.todo-item-edit{
  font-size: 1.5rem;
  color: #2c3e50;
  margin-left: 1rem;
  background: transparent;
  padding: 12px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Arial, Helvetica, sans-serif;
  font-style: italic;
  &:focus{
    outline: none;
  }
}
.completed{
  background-color: #F9F9F9;
  border-left: 10px solid #FF9B42;
}
#checkbox{
  padding-left: 5px;
  margin-left: 10px;
}
.check-all{
  font-size: 1rem;
  color: #534D41;

  &:hover{
    color: #2c3e50;
  }
}
.input-bar{
  border: 1px solid #cacaca;
  margin-bottom: 20px;
}
.todo-list-title{
  text-align: center;
  margin-bottom: 30px;;
  font-size: 3rem;
  font-weight: 700;
}
.bottom-container{
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 1rem;
  border-top: 1px solid #cacaca;
  padding: 10px;
}
button{
  font-size: .7rem;
  background-color: white;
  appearance: none;
  margin: 1px;
  padding: 5px;
  border: 1px solid black;

  &:hover{
    background-color:  #FF9B42;
    color: #f9f9f9;
  }
  &:focus{
    outline: none;
  }
}
.active{
    background-color: #FF9B42;
    color: #f9f9f9;
  }
.clear-completed{
  cursor: pointer;
}
.fade-enter-active, .fade-leave-active{
  transition: opacity .7s;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
</style>
