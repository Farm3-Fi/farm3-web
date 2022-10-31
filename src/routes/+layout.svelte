<script>
  import "../app.postcss";
</script>

<main class=" h-screen flex flex-col overflow-x-hidden bg-gradient-to-b from-green-300 to-slate-100">
  <nav class=" bg-white mx-20 mt-6 grid justify-center items-center grid-cols-8 rounded-md  sticky top-0 z-10 h-14">
    <p class="col-span-5 pl-10 text-lg font-bold">Farm3</p>
    <p>Pools</p>
    <p>About</p>
    <button class="bg-orange-500 h-full rounded-r-md text-white">Connect Wallet</button>
  </nav>
  <slot />
</main>

