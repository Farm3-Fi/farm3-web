<script>
  import TransactionModal from "$lib/modals/transactionModal.svelte";
  import futuresABI from "$lib/abis/futuresABI.json";
  import {provider, wallet, modalData} from "$lib/web3Store";
  import {ethers} from "ethers";


  let openModal = false;
  const arr = ["Futures", "Base Price", "Total Supply", "Expiry Date"];
  const list = [
    "0xFdbEfDa8d1aF710cee0E6f76093D8A300B5EE1a7",
    "0xd73bc8aff52df9bb93b3d52ba463535d8ff55062"
  ];

  const tap = (data) => {
    $modalData = data;
    openModal = !openModal;
  }

  const getData = async (contract) => {
    const future = new ethers.Contract(contract, futuresABI, $provider);
    const name = await future.symbol();
    const supply = await future.totalSupply();
    const baseprice = await future.BASEPRICE();
    const expiryDate = await future.expiryDate();
    return {
      address: contract,
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
    
     {#await getData(item)}
        <p class="px-4 py-2">Loading...</p>
     {:then data} 
     <button
     on:click={()=>tap(data)}
      class="text-left w-full h-full grid grid-cols-4"
    >
     <p  class="px-6 py-4 font-semibold">{data.name}</p>
     <p class="px-6 py-4">{new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(data.baseprice.toString().substring(0,3))}</p>
     <p class="px-6 py-4">{new Intl.NumberFormat('en-US').format(data.supply/1000000)}</p>
     <p class="px-6 py-4">{new Date(data.expiry * 1000).toLocaleDateString()}</p>
</button>
     {/await}

  {/each}
</div>
{:else}
<p class="w-full text-center mt-4 text-black">Please connect Metamask</p>
{/if}
