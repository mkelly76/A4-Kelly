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
          important = false;
          normal = false;
          low = false;
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
<div class="agendazen">
     <h1>AgendaZen</h1>

     <form on:submit|preventDefault={addToArray}>
          <h2 class=label-wrapper>
               <label class="label__lg">New Task:</label>
          </h2>
          <input type="text" class="input__lg" bind:value={todoItem}>
          <h2 class=label-wrapper>
               <label class="label__lg">Importance:</label>
          </h2>
          <div class="importance">
               <input type="checkbox" name="urgent" id="urgent" bind:checked={urgent}>
               <label for="urgent" class="urgent">Urgent</label>
               <input type="checkbox" name="important" id="important" bind:checked={important}>
               <label for="important" class="important">Important</label>
               <input type="checkbox" name="normal" id="normal" bind:checked={normal}>
               <label for="normal" class="normal">Normal</label>
               <input type="checkbox" name="low" id="low" bind:checked={low}>
               <label for="low" class="low">Low</label>
               <input type="checkbox" name="someday" id="someday" bind:checked={someday}>
               <label for="someday" class="someday">Someday</label>
          </div>
          <div>
               <button type="submit" class="btn btn__primary btn__lg">ADD</button>
          </div>
          
     </form>

     <ul role="list" class="todoList">
          {#each $todoList as item, index}
               <li class:urgent={item.urgent} class:important={item.important} class:normal={item.normal} class:low={item.low} class:someday={item.someday}>
                    <input type="checkbox" bind:checked={item.done} on:change={updateList}>
                    <span class:done={item.done}>{item.text}</span>
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <span on:click={() => removeThis(index)} 
                    class="remove" role="button" aria-pressed="false" tabindex="0"><span class="visually-hidden">Remove This</span>&times;</span>
               </li>
          {/each}
     </ul>

     {#if isDone.length > 0}
     <button on:click={clearDone} class="removeBtn">REMOVE COMPLETED</button>
     {/if}
</div>

<style>
     .agendazen {
          background: #ffffffb7;
          margin: 2rem 0 4rem 0;
          padding: 1rem;
          max-width: max-content;
          box-shadow:
               0 2px 4px 0 rgb(0 0 0 / 20%),
               0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
          margin-left: auto;
          margin-right: auto;
     }
     button {
          border: none;
          margin: 0;
          padding: 0;
          width: auto;
          overflow: visible;
          background: #bb00ff36;
          color: inherit;
          font: inherit;
          line-height: normal;
          -webkit-font-smoothing: inherit;
          -moz-osx-font-smoothing: inherit;
          appearance: none;
          font-weight: 700;
     }
     .btn {
          border: 1px solid darkslategrey;
          padding: 2px;
          cursor: pointer;
          display: inline-block;
          width: 100%;
          font-size: 1.1rem;
          box-shadow:
               0 2px 4px 0 rgb(47 79 79 / 20%),
               0 2.5rem 5rem 0 rgb(47 79 79 / 10%);
     }
     .removeBtn {
          border: 1px solid darkslategrey;
          padding: 2px;
          margin: 0 0 2vw 0;
          cursor: pointer;
          display: inline-block;
          font-size: 0.8rem;
          width: 50%;
          box-shadow:
               0 2px 4px 0 rgb(47 79 79 / 20%),
               0 2.5rem 5rem 0 rgb(47 79 79 / 10%);
     }
     ul {
          list-style: none;
          padding: 0;
     }
     li {
          font-size: 1.5rem;
          font-weight: 300;
          font-style: italic;
          border-bottom: rgba(47, 79, 79, 0.24) solid 0.25px;
     }
     .done {
          color: darkslategrey;
          text-decoration: line-through;
     }
     .remove {
          color: darkslategrey;
          cursor: pointer;
          float: right;
     }
     .urgent {
          color: #FF0000;
     }
     .important {
          color: #FF5C00;
     }
     .normal {
          color: #001AFF; 
     }
     .low {
          color: #039200;
     }
     .someday {
          color: #BD00FF;
     }
     .importance {
          padding: 0 0 1.5vw 0;
     }

</style>