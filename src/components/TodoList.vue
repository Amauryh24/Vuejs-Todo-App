<template>
  <div>
    <input type="text" class="todo-input" placeholder="what needs to be done" v-model="newTodo"
      @keyup.enter="addTodo" />

    <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <input class="completed-item" type="checkbox" v-model="todo.completed">
        <div v-if="!todo.editing" @click="editTodo(todo)" class="todo-item-label"
          :class="{ completed : todo.completed }">{{ todo.title }}</div>
        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
    </div>
    <div class="extra-container">
      <div>
        <label for="">
          <input type="checkbox"> check All
        </label>
        <div>{{ remaining }} items left</div>
      </div>
    </div>
  </div>

</template>

<script>
  export default {
    name: "todo-list",
    data() {
      return {
        newTodo: "",
        idForTodo: 3,
        beforeEditCache: '',
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
      }

    }
  };

</script>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  html {
    width: 100%;
    height: 100%;
    background: rgb(77, 184, 128);
    background: linear-gradient(157deg, rgba(77, 184, 128, 1) 0%, rgba(69, 145, 116, 1) 52%, rgba(53, 73, 94, 1) 100%);
    background-repeat: no-repeat;
    background-size: cover;

  }



  .todo-input {
    width: 100%;
    padding: 10px 20px;
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

  .todo-item-edit:hover {
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

</style>
