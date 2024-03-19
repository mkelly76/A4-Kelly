<script>
     import '../style.css'
     let todoItem = '';
     let todoList = [];
     $: isDone = todoList.filter(item => item.done);
     function addToArray() {
          if (todoItem == '') {
               return;
          }
          //todoList.push(todoItem);
          //todoList = todoList;
          todoList = [...todoList, {
               text: todoItem,
               done: false
          }];
          console.log(todoList);
          todoItem = '';
     }
     function removeThis(index) {
          todoList.splice(index, 1);
          todoList = todoList;
     }
     function clearDone() {
          todoList = todoList.filter(item => !item.done)
     }
</script>

<h1>AgendaZen</h1>

<form on:submit|preventDefault={addToArray}>
     <input type="text" bind:value={todoItem}>
     <button type="submit">Add</button>
</form>

<ul>
     {#each todoList as item, index}
          <li>
               <input type="checkbox" bind:checked={item.done}>
               <span class:done={item.done}>{item.text}</span>
               <span on:click={() => removeThis(index)} 
               class="remove" role="button" tabindex="0">&times;</span>
          </li>
     {/each}
</ul>

{#if isDone.length > 0}
<button on:click={clearDone}>Remove Done</button>
{/if}

<style>
     ul {
          list-style: none;
     }
     li {
          font-size: 1.3rem;
     }
     .done {
          color: #ccc;
          text-decoration: line-through;
     }
     .remove {
          color: darkred;
          cursor: pointer;
     }
</style>