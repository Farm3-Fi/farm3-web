<script>
    const arr = [{title:"Wheat"}, {title:"Grape"}]
</script>

<div class="w-full h-full grid grid-cols-4 gap-3 mx-20 mt-6">
    {#each arr as item}
    <div class="bg-white bg-opacity-25 rounded-md p-6 h-20">{item.title}</div>
{/each}
</div>
