<template>
    <div>
        <router-link to="/">Home</router-link>
        <hr>
        <addTodo
                @add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not completed</option>
        </select>
        <hr>
        <Loader v-if="loading"/>
        <TodoList
                v-else-if="filteredTodos.length"
                v-bind:todos="filteredTodos"
                v-on:remove-todo="removeTodo"
        />
        <p v-else>No todos</p>
    </div>
</template>


<script>
    import TodoList from '@/components/TodoList';
    import addTodo from '@/components/addTodo';
    import Loader from '@/components/Loader';
    export default {
        name: 'app',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        async mounted() {
//            fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
//                .then(response => response.json())
//                .then(json => {
//                    this.todos = json;
//                    this.loading = false;
//                });

            const res = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=3');
            const todos = await res.json();
            this.todos = todos;
            this.loading = false;
        },
        components: {
            TodoList, addTodo, Loader
        },
        computed: {
          filteredTodos() {
              if (this.filter === 'all') {
                  return this.todos
              } else if (this.filter === 'completed') {
                  return this.todos.filter(t => t.completed)
              } else {
                  return this.todos.filter(t => !t.completed)
              }
          }
        },
        methods: {
            removeTodo(id) {
                this.todos = this.todos.filter(t => t.id !== id);
            },
            addTodo(todo) {
                this.todos.push(todo);
            }
        }
    }
</script>
