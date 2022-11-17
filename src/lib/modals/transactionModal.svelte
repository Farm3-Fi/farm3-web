<script>
    export let isOpen = false;
    let symbol = "WHEAT";
    let expiryDate = Date.now();
    let supply = 1000;
  let address = "0x81237123193NANDAd812e12";
  let price = 3000;
</script>

{#if isOpen}
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div
on:click={() => isOpen = !isOpen}
  class="h-full w-full text-black flex justify-center items-center bg-black/60 absolute top-0 left-0 z-50"
>
  <div class="h-[640px] w-[480px] bg-white rounded-2xl p-4 flex flex-col">
    <h1 class="font-bold text-2xl">Buy Future ({symbol})</h1>
    <h2 class="text-neutral-400 text-sm">Future Address</h2>
    <h2 class="text-sm underline">{address}</h2>
    <div class="grid grid-cols-2 border-y py-8 mt-10 gap-4">
        <div>
            <p class="font-semibold">LIVE Commodity Price</p>
            <p>{price}</p>
          </div>
          <div>
            <p class="font-semibold">Future Price</p>
            <p>{price}</p>
          </div>
          <div>
            <p class="font-semibold">Expiry Date</p>
            <p>{new Date(expiryDate).toLocaleDateString("en-US")}</p>
          </div>
          <div>
            <p class="font-semibold">Total {symbol} Supply</p>
            <p>{supply}</p>
          </div>
          <div>
            <p class="font-semibold">Remaining {symbol} Supply</p>
            <p>{supply}</p>
          </div>
          <div>
            <p class="font-semibold">Farmer</p>
            <p>address</p>
          </div>
    </div>
    <div class="mt-4 flex flex-col h-full w-full">
        <p class="text-sm text-neutral-700 pb-2">Enter Amount to Buy</p>
        <input class="border rounded-full p-2 w-full" type="number">
        <p class="text-right w-full">2,300 USD</p>
        <div class="flex-grow"></div>
    <button class="w-full mt-3 bg-black text-white py-3 rounded-full">Buy {symbol}</button>
    </div>
  </div>
</div>
{/if}
