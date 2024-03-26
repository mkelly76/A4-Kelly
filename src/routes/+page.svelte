<script>
     import '../style.css';
     import { writable } from 'svelte/store';
     let todoItem = '';
     let storedList;
     let urgent, important, normal, low, someday; 
     let todoList = writable([]);

     if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
          storedList = localStorage.getItem('storedList');
          if(storedList) {
               $todoList = (JSON.parse(storedList));
          }
     }

     function updateList() {
          return storedList = localStorage.setItem('storedList', JSON.stringify($todoList));
     }

     $: isDone = $todoList.filter(item => item.done);
     $: somedayList = $todoList.filter(item => item.someday);

     function addToArray() {
          if (todoItem == '') {
               return;
          }
          //todoList.push(todoItem);
          //todoList = todoList;
          $todoList = [...$todoList, {
               text: todoItem,
               done: false,
               urgent: urgent,
               important: important,
               normal: normal,
               low: low,
               someday: someday
          }];
          console.log($todoList);
          updateList();
          todoItem = '';
          urgent = false;
          someday = false;
     }
     function removeThis(index) {
          $todoList.splice(index, 1);
          $todoList = $todoList;
          updateList();
     }
     function clearDone() {
          $todoList = $todoList.filter(item => !item.done)
          updateList();
     }
</script>

<h1>AgendaZen</h1>

<form on:submit|preventDefault={addToArray}>
     <input type="text" bind:value={todoItem}>
     <p>Choose Category</p>
     <input type="checkbox" name="urgent" id="urgent" class="urgent" bind:checked={urgent}>
     <label for="urgent">Urgent</label>
     <input type="checkbox" name="important" id="important" bind:checked={important}>
     <label for="important">Important</label>
     <input type="checkbox" name="normal" id="normal" bind:checked={normal}>
     <label for="normal">Normal</label>
     <input type="checkbox" name="low" id="low" bind:checked={low}>
     <label for="low">Low</label>
     <input type="checkbox" name="someday" id="someday" bind:checked={someday}>
     <label for="someday">Someday</label>
     <div>
          <button type="submit">Add</button>
     </div>
     
</form>

<ul>
     {#each $todoList as item, index}
          <li class:urgent={item.urgent} class:low={item.low}>
               <input type="checkbox" bind:checked={item.done} on:change={updateList}>
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
     .urgent {
          color: red;
     }
     .low {
          color: greenyellow;
     }
</style>