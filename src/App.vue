<template>
  <div class="container py-5">
    <div class="row d-flex justify-content-center">
      <div class="col-md-6">
        <div class="card mb-3">
          <div class="card-header">
            <h5 class="card-title">Simple Todo App</h5>
          </div>
          <div class="card-body">
            <div class="row">
              <div class="col-md-10">
                <input type="email" class="form-control" id="floatingInput" placeholder="Input todo list ..."
                  v-model="todo"
                  @keypress.enter="addTodo">
              </div>
              <div class="col-md-2">
                <button class="btn btn-primary w-100" @click="addTodo">+ Add</button>
              </div>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-body">
            <h5 class="card-title" v-if="todos.length == 0">Empty todo list ...</h5>
            <ol class="list-group" v-for="todo in todos" :key="todo.id">
              <li class="list-group-item"
                :class="{ 'text-decoration-line-through': todo.isDone, 'bg-warning': todo.isDone }">
                {{ todo.todo }}
                <a href="#" class="btn btn-sm btn-danger float-end" @click="removeTodo(todo.id)">X</a>
                <a href="#" class="btn btn-sm btn-success float-end me-1" @click="doneTodo(todo.id)">V</a>
              </li>
            </ol>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        todo: '',
        todos: [],
      }
    },
    methods: {
      addTodo() {
        let todoString = this.todo
        if (todoString.replace(/\s/g,"") == "") {
          alert('Input todo list cannot be empty!')
          return false
        }

        let incrementalLastTodoId = this.todos.length > 0 ? this.todos[this.todos.length - 1].id + 1 : 1
        this.todos.push( {id: incrementalLastTodoId, todo: this.todo, isDone: false} )
        this.todo = ''
      },
      removeTodo(id) {
        this.todos = this.todos.filter((todo) => {
          return todo.id != id
        })
      },
      doneTodo(id) {
        this.todos.filter((todo) => {
          if (todo.id == id) {
            return todo.isDone = !todo.isDone
          }
        })
      }
    },
    updated() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    },
    created() {
      this.todos = JSON.parse(localStorage.getItem('todos'))
    }
  }
</script>