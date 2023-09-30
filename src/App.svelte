<script>
	import { onMount } from "svelte";

	let newTodoData = ""
	let todoList = []
	let editMode = false
	let editId = 0

	function createTodo() {
		if (newTodoData !== "") {
			const newTodo = {
				id: new Date().getTime(),
				data: newTodoData,
				status: false
			}

			todoList.push(newTodo)
			todoList = todoList
			newTodoData = ""

			saveData()
		}
	}

	function updateTodo() {
		editMode = !editMode

		if (newTodoData !== "") {
			let editableTodoIndex = todoList.findIndex((todo) => todo.id === editId)
			
			todoList[editableTodoIndex].data = newTodoData
			editId = 0
			newTodoData = ""

			saveData()
		}
	}

	function updateTodoHelper(id) {
		editMode = !editMode
		editId = id
	}

	function deleteTodo(id) {
		todoList = todoList.filter((todo) => todo.id !== id)
		saveData()
	}

	function saveData() {
		localStorage.setItem("todoList", JSON.stringify(todoList))
	}

	function loadData() {
		const savedData = localStorage.getItem("todoList");
    	if (savedData) {
        	todoList = JSON.parse(savedData);
    	}
	}

	onMount(loadData)
</script>

<main>
	<h2>TO-DO Application</h2>

	<input bind:value = { newTodoData } type="text">

	{ #if editMode === true }
		<button on:click = { updateTodo }>Edit</button>
	{ :else }
		<button on:click = { createTodo }>Add</button>
	{ /if }

	<br>
	<div>
		<ul>
			{ #each todoList as todo }
				<li>
					<input bind:checked = { todo.status } on:change = { saveData }  type="checkbox" name="" id="">
					<span>{ todo.data }</span>
					<button on:click = { deleteTodo(todo.id) }>Delete</button>
					<button on:click = { updateTodoHelper(todo.id) }>Edit</button>
				</li>
			{ /each }
		</ul>
	</div>
</main>

<style>
	
</style>