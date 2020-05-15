<template>
  <div>
		<router-link to="/home">Home</router-link>
		<hr>
		<AddTodoItem
			@add-todo='addTodo'
		/>
		<hr>
			<select v-model="filter">
				<option value="all">All</option>
				<option value="completed">Completed</option>
				<option value="not-completed">Not Completed</option>
			</select>
		<hr>
		<Loader
			v-if="loading"
		></Loader>
		<TodoList
			v-else-if="filterTodos.length"
			v-bind:todos='filterTodos'
			@remove-todo='removeTodo'
		></TodoList>
		<p v-else>There are not todos yet</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import AddTodoItem from '@/components/AddTodoItem'
import Loader from '@/components/Loader'

export default {
	name: 'App',
		data() {
		return {
			todos: [],
			loading: true,
			filter: 'all'
		}
	},
	// watch: {
	// 	filter(value) {
	// 		console.log(value)
	// 	}
	// },
	computed: {
		filterTodos() {
			const me = this;
			if (me.filter === 'all') {
				return me.todos
			}
			if (me.filter === 'completed') {
				return me.todos.filter(t => t.completed)
			}
			if (me.filter === 'not-completed') {
				return me.todos.filter(t => !t.completed)
			}
			return me.todos
		}
	},
	mounted() {
		const me = this;
		fetch('https://jsonplaceholder.typicode.com/todos?_limit=4')
			.then(response => response.json())
			.then(json => {
				setTimeout(() => {
					me.todos = json;
					me.loading = false
				}, 1500)

			})
	},
  components: {
		TodoList,
		AddTodoItem,
		Loader
	},
	methods: {
		removeTodo(id) {
			let me = this;
			me.todos = me.todos.filter( t => t.id !== id)
		},
		addTodo(todo) {
			let me = this;
			me.todos.push(todo)
		}
	}
}
</script>