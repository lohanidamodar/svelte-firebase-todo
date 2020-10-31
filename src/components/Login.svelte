<script>
  import Profile from "./Profile.svelte";
  import Todos from "./Todos.svelte";
  import { auth, googleProvider } from "../firebase";
  import { authState } from "rxfire/auth";
  let user = authState(auth);
  function login() {
    auth.signInWithPopup(googleProvider);
  }
</script>

<section>
  {#if $user}
    <Profile {...$user} />
    <button
      class="bg-pink-600 hover:bg-pink-500 py-2 px-4 font-bold text-white rounded rounded-t-none"
      on:click="{() => auth.signOut()}">
      Logout
    </button>
    <div class="mt-4">

      <Todos uid="{$user.uid}" />
    </div>
  {:else}
    <div class="bg-pink-600 p-4 rounded text-white text-center">
      <h2 class="font-bold font-xl mb-2">Welcome to Svelte Firebase Todos</h2>
      <button
        class="py-2 px-4 bg-white text-pink-600 rounded hover:bg-gray-200"
        on:click="{login}">
        Signin With Google
      </button>
    </div>
  {/if}
</section>
