<script>
  import "../app.postcss";
</script>

<main class=" h-screen flex flex-col overflow-x-hidden bg-yellow-400 text-white">
  <nav class="mx-10 mt-6 flex justify-between items-center rounded-md  sticky top-0 z-10 h-14">
    <p class="col-span-5 text-3xl font-bold">HoneyLemon</p>
    <button class="bg-white px-10 h-full rounded-full text-yellow-400">Connect Wallet</button>
  </nav>
  <div class="mx-10 mt-8 text-neutral-500">
    <slot />
  </div>
</main>

