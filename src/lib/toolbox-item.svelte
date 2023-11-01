<script>
    import { stringify } from "postcss";
import "../style.css";

    import { createEventDispatcher } from 'svelte'

    export let toolboxItem;
    const dispatch = createEventDispatcher()

function starClicked(item) {
  dispatch('starClicked', toolboxItem)
}

</script>
<div class="flex flex-col gap-10 card card-hover">
    <div class="flex flex-row justify-between">
        <span class="font-extrabold">{ toolboxItem.title }</span>
        <svg on:click={starClicked} class:not-starred={!toolboxItem.starred} class:starred={toolboxItem.starred}  
        class="star">
            <use href="./star.svg#star"></use>
        </svg>
    </div>
    <div>
        { toolboxItem.subTitle}
    </div>
</div>

<style lang="postcss">
    .star {
        @apply w-[32px] h-[32px] stroke-[#ED8A19] transition-all;
    }

    .starred {
        @apply fill-[#ED8A19]
    }

    .not-starred {
        @apply fill-white hover:fill-[#ED8A19]

    }
</style>