<script>
import {TodoStore} from '../stores';
import { Table, Button } from 'sveltestrap';
import {createEventDispatcher, onMount} from 'svelte';
import axios from 'axios';
import { fade } from 'svelte/transition';

const endpoint = "http://localhost:3000/todos/";

onMount(async () => {
  try {
    const res = await axios.get(endpoint);
    TodoStore.update(() => {
      return res.data;
    });
  }
  catch(e){}
});

const dispatch = createEventDispatcher();

const handleEdit = (todo) => {   
    dispatch('edit-todo', todo)
};

const handleDelete = async (todoId) => {
    await axios.delete(endpoint + todoId);
    
    TodoStore.update(currentTodos => {
	    return currentTodos.filter(todo => todo.id != todoId)    
    });
};

const toggleComplete = async (todo) => {   
  todo.complete = !todo.complete;
  todo.dateCompleted = new Date().toDateString();

  await axios.patch(endpoint+todo.id,{
    complete:todo.complete, 
    dateCompleted: todo.dateCompleted
  });

  TodoStore.update(currentTodos => {
    const updatedToDoIndex = currentTodos.findIndex(
      t => t.id === todo.id
    );

    const updatedToDos = [
      ...currentTodos.slice(0,updatedToDoIndex),
      todo,
      ...currentTodos.slice(updatedToDoIndex + 1)
    ];

    return updatedToDos;
  });    
};
</script>

<Table>
    <thead>
      <tr>        
        <th>To Do</th>
        <th>Date Created</th>
        <th>Complete</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
    </thead>
    <tbody> 
      {#each $TodoStore as todo (todo.id)}
        {#if !todo.complete}
          <tr transition:fade>
            <td>{todo.text}</td>
            <td>{todo.date}</td>
            <td>
              <Button on:click={() => toggleComplete(todo)} color='success'>
                Mark Complete
              </Button>  
            </td>            
            <td>
              <Button on:click={() => handleEdit(todo)} color='warning'>
                  Edit
              </Button>
            </td>
            <td>
              <Button on:click={() => handleDelete(todo.id)} color='danger'>
                  Delete
              </Button>    
            </td>        
          </tr>
        {/if}
      {/each}                     
    </tbody>
</Table>

<br>
<h1>Completed Todos</h1>
<Table>
  <thead>
    <tr>        
      <th>To Do</th>
      <th>Date Completed</th>
      <th>Complete</th>        
      <th>Delete</th>
    </tr>
  </thead>
  <tbody>      
    {#each $TodoStore as todo (todo.id)}
      {#if todo.complete}
        <tr>            
          <td>{todo.text}</td>
          <td>{todo.dateCompleted}</td>
          <td>
            <Button on:click={() => toggleComplete(todo)}>
              Mark Uncomplete                  
            </Button>
          </td>              
          <td><Button on:click={() => handleDelete(todo.id)} color='danger'>Delete</Button></td>                                 
        </tr>             
      {/if} 
    {/each}  
  </tbody>
</Table>