<script>
	import ToDoList from './components/ToDoList.svelte';
	import ToDoForm from './components/ToDoForm.svelte';
	import ToDoEditForm from './components/ToDoEditForm.svelte';
	import { TodoStore } from './stores';

	$: count = $TodoStore.length;
	$: pendingTodosCount = $TodoStore.filter(
			todo => {return !todo.complete}
		).length;
	$: completedTodosCount = $TodoStore.filter(
			todo => {return todo.complete}
		).length;

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
	<br />
	<h4>Total Todos: {count} | Pending: {pendingTodosCount} | Completed: {completedTodosCount}</h4>
	<br />
	<ToDoList on:edit-todo={editTodoEvent} />
</main>