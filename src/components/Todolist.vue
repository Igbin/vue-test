<template>
    <div>
        <ul>
           <TodoItem
                   v-for="(todo, index) of todos"
                   v-bind:todo="todo"
                   v-bind:key="todo.id"
                   v-bind:index="index"
                   v-on:remove-todo="removeTodo"
           />
        </ul>
    </div>
</template>

<script>
    import TodoItem from "@/components/TodoItem"
    export default {
        props: ['todos'],
        components: {
            TodoItem
        },
        methods: {
            removeTodo(id) {
                this.$emit('remove-todo', id)
            }
        }
    }
</script>

<style>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }
</style>