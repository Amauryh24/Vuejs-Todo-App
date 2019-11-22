<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!anyRemaining">
      </todo-item>
    </transition-group>

    <div class="extra-container">
      <todo-check-all></todo-check-all>
      <todo-items-remaining></todo-items-remaining>
    </div>

    <div class="extra-container">
      <todo-filtered></todo-filtered>

      <div>
        <transition name="fade">
          <todo-clear-completed :showClearCompletedButton="showClearCompletedButton"></todo-clear-completed>
        </transition>
      </div>

    </div>
  </div>
</template>

<script>
  import TodoItem from './TodoItem'
  import TodoItemsRemaining from './TodoItemsRemaining'
  import TodoCheckAll from './TodoCheckAll'
  import TodoFiltered from './TodoFiltered'
  import TodoClearCompleted from './TodoClearCompleted'
  export default {
    name: 'todo-list',
    components: {
      TodoItem,
      TodoItemsRemaining,
      TodoCheckAll,
      TodoFiltered,
      TodoClearCompleted,
    },
    data() {
      return {
        newTodo: '',
        idForTodo: 3,
        filter: 'all',
        todos: [{
            'id': 1,
            'title': 'Finish Vue Screencast',
            'completed': false,
            'editing': false,
          },
          {
            'id': 2,
            'title': 'Take over world',
            'completed': false,
            'editing': false,
          },
        ]
      }
    },
    created() {
      // eventBus.$on('removedTodo', (id) => this.removeTodo(id))
      eventBus.$on('finishedEdit', (data) => this.finishedEdit(data))
      eventBus.$on('checkAllChanged', (checked) => this.checkAllTodos(checked))
      eventBus.$on('filterChanged', (filter) => this.$store.state.filter = filter)
      eventBus.$on('clearCompletedTodos', () => this.clearCompleted())
    },
    beforeDestroy() {
      // eventBus.$off('removedTodo')
      eventBus.$off('finishedEdit')
      eventBus.$off('checkAllChanged')
      eventBus.$off('filterChanged')
      eventBus.$off('clearCompletedTodos')
    },
    computed: {
      remaining() {
        return this.$store.getters.remaining
      },
      anyRemaining() {
        return this.$store.getters.anyRemaining
      },
      todosFiltered() {
        return this.$store.getters.todosFiltered
      },
      showClearCompletedButton() {
        return this.$store.getters.showClearCompletedButton
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim().length == 0) {
          return
        }
        this.$store.state.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
        })
        this.newTodo = '';
        this.idForTodo++;
      },
      // removeTodo(id) {
      //   const index = this.$store.state.todos.findIndex(item => item.id == id)
      //   this.$store.state.todos.splice(index, 1)
      // },
      checkAllTodos() {
        this.$store.state.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted() {
        this.$store.state.todos = this.$store.state.todos.filter(todo => !todo.completed)
      },
      finishedEdit(data) {
        const index = this.$store.state.todos.findIndex((item) => item.id == data.id)
        this.$store.state.todos.splice(index, 1, data)
      }
    }
  }

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
