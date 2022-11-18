<script>
  import { modalData, provider } from "$lib/web3Store";
  import {ethers} from "ethers";
  import futuresABI from "$lib/abis/futuresABI.json";
  export let isOpen;
  let amt = 0;
  let approved = false;
  const stablecoin = "0xA8CB41Ac920f267a4696F06e7C9575dcF6258d26"

  const approve = async () => {
    const stable = new ethers.Contract(stablecoin, futuresABI, $provider);
    const stableSigner = stable.connect($provider.getSigner())
    const res = await stableSigner.approve($modalData.address, ethers.BigNumber.from("1000000"));
    if(res){
      console.log(res)
      approved = true;
    }
    
  }

  const buy = async () => {
    const future = new ethers.Contract($modalData.address, futuresABI, $provider);
    const futureSigner = future.connect($provider.getSigner())
    const res = await futureSigner.buyFuture(ethers.BigNumber.from(amt));
    console.log(res)
  }

  const getData = async () => {
    const future = new ethers.Contract($modalData.address, futuresABI, $provider);
    const farmer = await future.farmer();
    const remaining = await future.balanceOf($modalData.address);
    return {
      farmer: farmer,
      remaining: remaining
    };
  }
</script>

{#if isOpen}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div
    on:click={() => (isOpen = !isOpen)}
    class="h-full w-full text-black flex justify-center items-center bg-black/60 absolute top-0 left-0 z-50"
  >
    <div
      on:click|stopPropagation
      class="h-[640px] w-[480px] z-20 bg-white rounded-2xl p-4 flex flex-col"
    >
      <h1 class="font-bold text-2xl">Buy Future {$modalData.name}</h1>
      <h2 class="text-neutral-400 text-sm">Future Address</h2>
      <a href="https://polygonscan.com/address/{$modalData.address}" class="text-sm underline">{$modalData.address}</a>
      <div class="grid grid-cols-2 border-y py-8 mt-10 gap-4">
        <div>
          <p class="font-semibold">LIVE Commodity Price</p>
          <p>
            {new Intl.NumberFormat("en-US", {
              style: "currency",
              currency: "USD",
            }).format($modalData.baseprice.toString().substring(0, 3))}
          </p>
        </div>
        <div>
          <p class="font-semibold">Future Price</p>
          <p>
            {new Intl.NumberFormat("en-US", {
              style: "currency",
              currency: "USD",
            }).format($modalData.baseprice.toString().substring(0, 3))}
          </p>
        </div>
        <div>
          <p class="font-semibold">Expiry Date</p>
          <p>
            {new Date($modalData.expiry * 1000).toLocaleDateString("en-US")}
          </p>
        </div>
        <div>
          <p class="font-semibold">Total {$modalData.name} Supply</p>
          <p>
            {new Intl.NumberFormat("en-US").format($modalData.supply / 1000000)}
          </p>
        </div>
        <div>
          <p class="font-semibold">Remaining {$modalData.name} Supply (Metric Ton)</p>
          {#await getData()}
            <p>....</p>
          {:then data}
          <p>
            {new Intl.NumberFormat("en-US").format(data.remaining / 1000000)}
          </p>
          {/await}
          
        </div>
        <div>
          <p class="font-semibold">Farmer</p>
          {#await getData()}
            <p>....</p>
          {:then data}
            <a href="https://polygonscan.com/address/{data.farmer}" class="underline">{data.farmer.substring(0, 8)}...</a>
          {/await}
        </div>
      </div>
      <div class="mt-4 flex flex-col h-full w-full">
        <p class="text-sm text-neutral-700 pb-2">Enter Amount to Buy</p>
        <input
          bind:value={amt}
          class="border z-50 rounded-full p-2 w-full"
          type="number"
        />
        <p class="text-right w-full">
          USD {$modalData.baseprice.toString().substring(0, 3) * amt}
        </p>
        <div class="flex-grow" />
        {#if !approved}
        <button on:click={approve} class="w-full mt-3 bg-black text-white py-3 rounded-full"
          >{"Approve " + $modalData.name}</button
        >
        {:else}
        <button on:click={buy} class="w-full mt-3 bg-black text-white py-3 rounded-full"
          >{"Buy " + $modalData.name}</button
        >
        {/if}
      </div>
    </div>
  </div>
{/if}
