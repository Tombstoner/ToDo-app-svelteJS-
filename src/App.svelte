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
	<h2 class="heading">TO-DO Application</h2>
  
	<div class="input">
		<input bind:value={newTodoData} type="text">
		{#if editMode === true}
			<button class="edit-button" on:click={updateTodo}>Edit</button>
		{:else}
			<button class="add-button" on:click={createTodo}>Add</button>
		{/if}
	</div>
  
	<br>

	<div class="todo-container">
		<ul>
			{#each todoList as todo}
				<li class="todo-item">
		  			<input bind:checked={todo.status} on:change={saveData} type="checkbox">
		  			<span>{todo.data}</span>
		  			<div class="buttons-container">
						<button class="delete-button" on:click={() => deleteTodo(todo.id)}>Delete</button>
						<button class="edit-button" on:click={() => updateTodoHelper(todo.id)}>Edit</button>
		  			</div>
				</li>
			{/each}
	  	</ul>
	</div>
</main>

<style>
	* {
	  margin: 0;
	  padding: 0;
	  box-sizing: border-box;
	}
  
	main {
	  font-family: Arial, sans-serif;
	}
  
	.heading {
	  text-align: center;
	  font-size: 24px;
	  margin: 20px 0;
	  color: #fff;
	}

	input[type="text"] {
	  width: 20%;
	  padding: 10px;
	  font-size: 16px;
	  border: none;
	  outline: none;
	  border-radius: 5px;
	  background-color: #ccccccb1;
	}

	.input {
		display: flex;
		justify-content: center;
	}
  
	.add-button,
	.edit-button {
	  padding: 10px 20px;
	  font-size: 16px;
	  margin-left: 10px;
	  cursor: pointer;
	  background-color: #007bff;
	  color: #fff;
	  border: none;
	  border-radius: 5px;
	}
  
	.add-button:hover,
	.edit-button:hover {
	  background-color: #0056b3;
	}
  
	ul {
	  list-style: none;
	  padding: 0;
	}
  
	.todo-item {
	  display: flex;
	  align-items: center;
	  border: none;
	  background-color: #cccccc53;
	  border-radius: 5px;
	  padding: 10px;
	  margin: 10px auto;
	  width: 40%;
	  color: #000;
	}
  
	input[type="checkbox"] {
	  margin-right: 10px;
	}
  
	span {
	  flex-grow: 1;
	  font-size: 18px;
	}
  
	.buttons-container {
	  display: flex;
	  gap: 10px;
	}
  
	.edit-button,
	.delete-button {
	  padding: 5px 10px;
	  font-size: 14px;
	  cursor: pointer;
	  border: none;
	  border-radius: 5px;
	}
  
	.edit-button {
	  background-color: #ffc107;
	  color: #000;
	}
  
	.edit-button:hover {
	  background-color: #ffab00;
	}
  
	.delete-button {
	  background-color: #dc3545;
	  color: #fff;
	}
  
	.delete-button:hover {
	  background-color: #c82333;
	}
</style>