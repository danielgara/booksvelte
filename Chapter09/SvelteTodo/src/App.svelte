<script>
	import ToDoList from './components/ToDoList.svelte';
	import ToDoForm from './components/ToDoForm.svelte';
	import ToDoEditForm from './components/ToDoEditForm.svelte';

	let editMode = false;
	let editTodo = null;

	const editTodoEvent = (event) => {	
		editMode = true;
		editTodo = event.detail;		
	};

	const finishEdit = (event) => {
		editMode = false;	
	};
</script>

<main>
	{#if editMode}
		<ToDoEditForm editTodo={editTodo} on:finish-edit={finishEdit} />
	{:else}
		<ToDoForm />	
	{/if}
	<ToDoList on:edit-todo={editTodoEvent} />
</main>