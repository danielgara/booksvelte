<script>
import {TodoStore} from '../stores';
import { Table, Button } from 'sveltestrap';
import {createEventDispatcher, onMount} from 'svelte';
import axios from 'axios';

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
        <tr>
          <td>{todo.text}</td>
          <td>{todo.date}</td>
          <td>Mark Complete</td>            
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
      {/each}                     
    </tbody>
</Table>