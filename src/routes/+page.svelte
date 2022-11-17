<script>
    import TransactionModal from "$lib/modals/transactionModal.svelte";
    let openModal = false;
    const arr = ["Futures", "Price", "Value", "Expiry Date"];
    const dummy = [{
        asset: "WHEAT", price:"500", value:"20000", expiryDate: Date.now()
    }, {
        asset: "WHEAT", price:"500", value:"20000", expiryDate: Date.now()
    }, {
        asset: "WHEAT", price:"500", value:"20000", expiryDate: Date.now()
    }]
</script>
<TransactionModal bind:isOpen={openModal}></TransactionModal>
<div class="w-full max-w-5xl mx-auto bg-neutral-800 rounded-xl mt-3 text-white">
    <div class="w-full h-full grid grid-cols-4">
        {#each arr as item}
        <h1 class="rounded-md px-6 py-4">{item}</h1>
        {/each}
    </div>
</div>

<div class="w-full max-w-5xl mx-auto bg-white rounded-xl divide-y mt-2 text-black">
    {#each dummy as item}
    <button on:click={()=> openModal = !openModal} class="text-left w-full h-full grid grid-cols-4">
        <p class="px-6 py-4 font-semibold">{item.asset}</p>
        <p class="px-6 py-4">{item.price}</p>
        <p class="px-6 py-4">{item.value}</p>
        <p class="px-6 py-4">{item.expiryDate}</p>
    </button>
    {/each}
</div>
