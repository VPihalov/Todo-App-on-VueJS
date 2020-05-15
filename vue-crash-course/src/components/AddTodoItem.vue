<template>
	<form @submit.prevent="onSubmit">
		<input type="text" v-model="title">
		<button type="submit">Create</button>
	</form>
</template>

<script>
export default {
		data() {
			return {
				title: ''
			}
		},
		methods: {
			onSubmit() {
				let me = this;
				if (me.title.trim()) {
					const todo = {
						id: Date.now(),
						title: me.title,
						completed: false
					}
					me.$emit('add-todo', todo);
					me.title = ''
				}
			}
		}
}
</script>

<style scoped>
	form {
		display: flex
	};
	input {
		width: 400px
	}
</style>