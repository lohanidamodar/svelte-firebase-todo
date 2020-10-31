<script>
  import { fade, fly } from "svelte/transition";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  function remove() {
    dispatch("remove", { id });
  }
  function toggleStatus() {
    let newStatus = !complete;
    dispatch("toggle", {
      id,
      newStatus,
    });
  }
  export let id;
  export let text;
  export let complete;
</script>

<li
  on:click="{toggleStatus}"
  class="p-4 w-full flex flex-row bg-gray-200 hover:bg-white p-2"
  in:fly="{{ x: 900, duration: 500 }}">
  <span class="block flex-grow">{text}</span>
  <div>
    {#if complete}
      <button on:click="{toggleStatus}">✔️</button>
    {:else}
      <button on:click="{toggleStatus}">❌</button>
    {/if}
    <button on:click="{remove}">🗑</button>
  </div>
</li>
