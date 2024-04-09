<script>
     import { fade, fly } from 'svelte/transition';
     import { bounceIn } from 'svelte/easing';
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
          important = false;
          normal = true;
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
<div class="agendazen tasks">
     <h1>AgendaZen</h1>

     <form on:submit|preventDefault={addToArray}>
          <h3 class=label-wrapper>
               <label class="label__lg">New Task:</label>
          </h3>
          <input type="text" class="input__lg" bind:value={todoItem}>
          <h3 class=label-wrapper>
               <label class="label__lg">Importance:</label>
          </h3>
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

     <!--<ul role="list" class="todoList">
          {#each $todoList as item, index}
               <li in:fly={{ y:10, duration: 500 }} eased = bounceIn(500) out:fade={{duration: 500}} class:urgent={item.urgent} class:important={item.important} class:normal={item.normal} class:low={item.low} class:someday={item.someday}>
                    <input type="checkbox" bind:checked={item.done} on:change={updateList}>
                    <span class:done={item.done}>{item.text}</span>-->
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <!--<span on:click={() => removeThis(index)} 
                    class="remove" role="button" aria-pressed="false" tabindex="0"><span class="visually-hidden">Remove This</span>&times;</span>
               </li>
          {/each}
     </ul>-->
</div>

{#if somedayList.length > 0}
<div class="agendazen somedayList" in:fly={{ y:10, duration: 500 }} eased = bounceIn(500) out:fade={{duration: 500}}>
     <h2 class="someday">Someday</h2>
     <ul>
          {#each somedayList as item, index}
               <li in:fly={{ y:10, duration: 500 }} eased = bounceIn(500) out:fade={{duration: 500}}>
                    <input type="checkbox" bind:checked={item.done} on:change={updateList}>
                    <span class:done={item.done}>{item.text}</span>
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <span on:click={() => removeThis(index)} class="remove" role="button" aria-pressed="false" tabindex="0"><span class="visually-hidden">Remove This</span>&times;</span>
               </li>
          {/each}
     </ul>
     {#if isDone.length > 0}
     <button on:click={clearDone} class="removeBtn">REMOVE COMPLETED</button>
     {/if}
</div>
{/if}




<style>
     .agendazen {
          background: #ffffffb7;
          margin: 1vw auto;
          padding: 1rem;
          max-width: 500px;
          box-shadow:
               0 2px 4px 0 rgb(0 0 0 / 20%),
               0 2.5rem 5rem 0 rgb(0 0 0 / 10%);
          border-radius: 3px;
     }
     .tasks {
          margin: 5vw auto 1vw;
     }
     .somedayList {
         outline: 4px dotted #bd00ff; 
         outline-offset: -5px;       
     }
     button {
          border: none;
          margin: 0.5em 0;
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
     .importance {
          padding: 2em 0;
          display: flex;
          justify-content: space-around;
     }
     .btn {
          border: 1px solid darkslategrey;
          padding: 0.5em;
          cursor: pointer;
          display: inline-block;
          width: 100%;
          font-size: 1.1rem;
          box-shadow:
               0 2px 4px 0 rgb(47 79 79 / 20%),
               0 2.5rem 5rem 0 rgb(47 79 79 / 10%);
          border-radius: 25px;
     }
     .removeBtn {
          border: 1px solid darkslategrey;
          padding: 0.5em;
          margin: 0 0 2vw 0;
          cursor: pointer;
          display: inline-block;
          font-size: 0.8rem;
          width: 50%;
          box-shadow:
               0 2px 4px 0 rgb(47 79 79 / 20%),
               0 2.5rem 5rem 0 rgb(47 79 79 / 10%);
          border-radius: 25px;
     }
     ul {
          list-style: none;
          padding: 0 1vw;
     }
     li {
          font-size: 1.5rem;
          font-weight: 300;
          font-style: italic;
          border-bottom: rgba(47, 79, 79, 0.24) solid 0.25px;
     }
     .somedayList li input[type="checkbox"] {
          width: 1.1em;
          height: 1.1rem;
          accent-color: #bd00ff;
     }
     .done {
          color: darkslategrey;
          text-decoration: line-through solid 1px;
          opacity: 0.5;
     }
     .remove {
          color: darkslategrey;
          cursor: pointer;
          float: right;
     }
     .urgent {
          color: #FF0000;
     }
     #urgent {
          accent-color: #ff0000;
     }
     .important {
          color: #FF5C00;
     }
     #important {
          accent-color: #ff5c00;
     }
     .normal {
          color: #001AFF; 
     }
     #normal {
          accent-color: #001AFF;
     }
     .low {
          color: #039200;
     }
     #low {
          accent-color: #039200;
     }
     .someday {
          color: #bd00ff;
          text-decoration-color: #bb00ff;
     }
     #someday {
          accent-color: #bd00ff;          
     }
     .importance {
          padding: 0 0 1.5vw 0;
     }

</style>