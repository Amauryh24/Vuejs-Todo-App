<template>
  <div>
    <input type="text" class="todo-input" placeholder="what needs to be done" v-model="newTodo"
      @keyup.enter="addTodo" />

    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index"
        @removedTodo="removeTodo">
        <!-- <div class="todo-item-left">
          <input class="completed-item" type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editing" @click="editTodo(todo)" class="todo-item-label"
            :class="{ completed : todo.completed }">{{ todo.title }}</div>
          <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div> -->
      </todo-item>
    </transition-group>

    <div class="extra-container">
      <label>
        <input class="completed-item" type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">
        Check All
      </label>
      <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button> </button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button> </button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button> </button>
      </div>
      <div>
        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="clearCompleted"> Clear completed</button>
        </transition>
      </div>
    </div>
  </div>

</template>

<script>
  import TodoItem from './TodoItem'
  export default {
    name: "todo-list",
    components: {
      TodoItem,
    },
    data() {
      return {
        newTodo: "",
        idForTodo: 3,
        beforeEditCache: '',
        filter: 'all',
        todos: [{
            id: 1,
            title: "Finish Vue Screencast",
            completed: false,
            editing: false,
          },
          {
            id: 2,
            title: "Take over world",
            completed: false,
            editing: false,
          }
        ]
      };
    },
    computed: {
      remaining() {
        return this.todos.filter(todo => !todo.completed).length
      },
      anyRemaining() {
        return this.remaining != 0
      },
      todosFiltered() {
        if (this.filter == 'all') {
          return this.todos
        } else if (this.filter == 'active') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter == 'completed') {
          return this.todos.filter(todo => todo.completed)
        }
        return this.todos
      },
      showClearCompletedButton() {
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
      addTodo() {
        if (this.newTodo.trim().length == 0) {
          return
        }

        this.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false
        })
        this.newTodo = '',
          this.idForTodo++
      },
      editTodo(todo) {
        this.beforeEditCache = todo.title
        todo.editing = true
      },
      doneEdit(todo) {
        if (todo.title.trim().length == '') {
          todo.title = this.beforeEditCache
        }
        todo.editing = false
      },
      cancelEdit(todo) {
        todo.title = this.beforeEditCache
        todo.editing = false
      },

      removeTodo(index) {
        this.todos.splice(index, 1)
      },

      checkAllTodos() {
        this.todos.forEach((todo) => todo.completed =
          event.target.checked)
      },
      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed)
      }

    }
  };

</script>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  @import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css');

  html {
    width: 100%;
    height: 100%;


  }



  .todo-input {
    width: 96%;
    padding: 1% 2%;
    font-size: 20px;
    margin-bottom: 20px;
    border-radius: 25px 25px 25px 25px;
    border: none;
    box-shadow: 0 0 0 2px #35495e,
      0 0 0 4px #4DB883,
      2px 2px 19px rgba(0, 0, 0, 0.24);
  }

  .todo-input:focus {
    outline: 0;
  }

  .todo-item {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.4s;
  }

  .remove-item {
    cursor: pointer;
    margin-left: 14px;

  }

  .remove-item:hover {
    color: black;
    transform: scale(1.5);
    transition-duration: 0.5s;

  }

  .todo-item-left {
    display: flex;
    align-items: center;
  }

  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
    border-radius: 25px 25px 25px 25px;
    border: none;
    box-shadow: 0 0 0 2px #35495e,
      0 0 0 4px #4DB883,
      2px 2px 19px rgba(0, 0, 0, 0.24);
  }

  .todo-item-edit {
    font-size: 24px;
    color: black;
    margin-left: 12px;
    width: 100%;
    Padding: 10px;

    border-radius: 25px 25px 25px 25px;
    border: none;
    box-shadow: 0 0 0 2px #35495e,
      0 0 0 4px #4DB883,
      2px 2px 19px rgba(0, 0, 0, 0.24);
  }

  .todo-item-edit:focus {
    outline: none;
  }

  .completed {
    text-decoration: line-through;
    background-color: rgba(77, 184, 131, 0.582);
    top: 50px;
    left: 50px;
  }

  .completed-item {
    transform: scale(1.5);
    cursor: pointer;
  }

  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgray;
    padding-top: 14px;
    margin-top: 14px;
  }

  button {
    border-radius: 25px;
    cursor: pointer;
    border: none;
    box-shadow:
      0 0 0 1px #4DB883,
      2px 2px 10px rgba(0, 0, 0, 0.24);
  }

  button:hover {
    background-color: #4DB883;
  }

  button:focus {
    outline: none;
  }

  button.active {
    background-color: #4DB883;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity .5s;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }

</style>
