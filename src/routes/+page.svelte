<script>
    import ToolboxItem from "$lib/toolbox-item.svelte";

    const items = [
        {id: "jwe-decrypt", title : "JWE body decryption", subTitle: "With symmetrical key", starred: true}
    ]

    function starClicked(event){
        const item = items.find(i => i.id === event.detail.id);
        if (!item) return;
        item.starred = item.starred;
    }

</script>
<div class="toolbox">
    {#each items as item}
      <div class="toolbox-item">
        <a href="/{item.id}">
            <ToolboxItem toolboxItem={item} on:starClicked={starClicked}></ToolboxItem>
        </a>
      </div>
    {/each}
</div>

<style lang="postcss">
    .toolbox {
        @apply flex flex-wrap justify-between gap-3 gap-10
    }

    .toolbox-item {
        @apply cursor-pointer w-full md:min-w-[500px] lg:max-w-[45%] shrink-0 h-[200px];
    }
</style>