<script>
    export let displayName;
    export let photoURL;
    export let uid;
</script>

<div class="p-4 bg-pink-600 rounded rounded-bl-none">
    <div class="border-8 border-pink-400 rounded-full w-20 h-20 overflow-hidden">
        <img src={photoURL} alt="user avatar">
    </div>
    <h3 class="font-semibold text-xl text-white">Hi, {displayName}!</h3>
</div>