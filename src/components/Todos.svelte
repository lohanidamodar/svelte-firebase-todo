<script>
  import TodoItem from "./TodoItem.svelte";
  import { db } from "../firebase";
  import { collectionData } from "rxfire/firestore";
  import { startWith } from "rxjs/operators";
  export let uid;
  const query = db
    .collection("todos")
    .where("uid", "==", uid)
    .orderBy("created");
  const todos = collectionData(query, "id").pipe(startWith([]));

  function updateStatus(event) {
    console.log(event);
    const { id, newStatus } = event.detail;
    db.collection("todos").doc(id).update({ complete: newStatus });
  }
  function removeItem(event) {
    const { id } = event.detail;
    db.collection("todos").doc(id).delete();
  }

  function add() {
    if (!text) return;
    db.collection("todos").add({
      text: text,
      complete: false,
      uid: uid,
      created: new Date(),
    });
    text = "";
  }

  let text;
</script>

<h3 class="font-bold text-md">Your Tasks</h3>
<ul class="py-4 bg-gray-400 w-full rounded shadow">
  {#each $todos as todo}
    <TodoItem {...todo} on:remove="{removeItem}" on:toggle="{updateStatus}" />
  {/each}
</ul>

<div class="flex p-2 flex-row bg-gray-200">
  <input
    class="p-2 rounded border-0 border-b-2 appereance-none focus:outline-none
    border-gray-400 focus:border-pink-600 active:border-pink-600 flex-grow"
    placeholder="enter you task"
    type="text"
    bind:value="{text}" />
  <button
    class="font-bold bg-pink-600 hover:bg-pink-500 rounded text-white px-4 ml-2"
    on:click="{add}">
    Add
  </button>
</div>
