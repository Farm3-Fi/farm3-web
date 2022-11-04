<script>
    const arr = ["Asset", "Price", "Value", "Expiry Date"];
</script>

<div class="w-full h-10 grid grid-cols-6">
    <div class="col-span-2"></div>
    <div class="col-span-2 bg-white rounded-full p-1">
        <div class="grid grid-cols-2 bg-neutral-100 h-full w-full rounded-full">
            <button class="bg-yellow-400 rounded-full text-white">Investor</button>
            <button>Farmer</button>
        </div>
    </div>
    <div class="col-span-2"></div>
</div>
<div class="w-full bg-white rounded-xl mt-3 ">
    <table class="w-full h-full border-b-2 border-neutral-400">
        {#each arr as item}
        <th class="bg-white bg-opacity-25 rounded-md p-6">{item}</th>
    {/each}
    </table>
</div>
