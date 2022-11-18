<script>
  import TransactionModal from "$lib/modals/transactionModal.svelte";
  import futuresABI from "$lib/abis/futuresABI.json";
  import {provider, wallet} from "$lib/web3Store";
  import {ethers} from "ethers";


  let openModal = false;
  const arr = ["Futures", "Price", "Value", "Expiry Date"];
  const list = [
    "0xFdbEfDa8d1aF710cee0E6f76093D8A300B5EE1a7",
    "0xd73bc8aff52df9bb93b3d52ba463535d8ff55062"
  ];

  const getData = async (contract) => {
    const future = new ethers.Contract(contract, futuresABI, $provider);
    const name = await future.symbol();
    const supply = await future.totalSupply();
    const baseprice = await future.BASEPRICE();
    const expiryDate = await future.expiryDate();
  
    return {
      name: name,
      supply: supply,
      baseprice: baseprice,
      expiry: expiryDate
    };
  }
</script>

<TransactionModal bind:isOpen={openModal} />
<div class="w-full max-w-5xl mx-auto bg-neutral-800 rounded-xl mt-3 text-white">
  <div class="w-full h-full grid grid-cols-4">
    {#each arr as item}
      <h1 class="rounded-md px-6 py-4">{item}</h1>
    {/each}
  </div>
</div>

{#if $wallet}
<div
  class="w-full max-w-5xl mx-auto bg-white rounded-xl divide-y mt-2 text-black"
>
  {#each list as item}
    <button
      on:click={() => (openModal = !openModal)}
      class="text-left w-full h-full grid grid-cols-4"
    >
     {#await getData(item)}
        <p>Loading...</p>
     {:then data} 
     <p class="px-6 py-4 font-semibold">{data.name}</p>
     <p class="px-6 py-4">{data.baseprice}</p>
     <p class="px-6 py-4">{data.supply * data.baseprice}</p>
     <p class="px-6 py-4">{data.expiry}</p>
     {/await}
    </button>
  {/each}
</div>
{:else}
<p class="w-full text-center mt-4 text-black">Please connect Metamask</p>
{/if}
