<script lang="ts">
    let { returnHome } = $props();
    import lineLayout from "./lineLayout.json"
    import slices from "$lib/assets/avocado-slice-background.png"

    let display = $state('full');
</script>

<div class="full flex horizontal line">
    <!-- {#each lineLayout as lineSection}
        <div class="flex" class:two-row-space={lineSection.length === 2} class:three-row-space={lineSection.length === 3}> -->
        <div class="flex three-row-space">
            {#each lineLayout as lineRow}
                <div class="flex horizontal line">
                    {#each lineRow as item}
                        {#if item.len === 1}
                            <div class="square"><div>{item.name}</div></div>
                        {/if}
                        {#if item.len === 2}
                            <div class="rect"><div style="background: url(/src/lib/assets/{item["image-name"]}) round;">{item.name}</div></div>
                        {/if}
                        {#if item.len === 0} 
                            <div class="divider"></div>
                        {/if}
                    {/each}
                </div>
            {/each}
        </div>
    <!-- {/each} -->
</div>

<button onclick={returnHome}>Return Home</button>

<style>
    :root {
        --border-size: 2px;
        --divider-size: 4px;
    }
    .full {
        width: 99vw;
        height: auto;
    }
    /* .full > div {
        align-content: stretch;
    } */
    .flex {
        display: flex;
        flex-direction: column;
    }
    .line {
        justify-content: space-between;
    }
    .horizontal {
        flex-direction: row;
    }
    .square {
        flex: 1;
        /* aspect-ratio: 1 / 1; */
    }
    .rect {
        flex: 2;
        padding: 0px var(--border-size) 0px var(--border-size) !important;
        background-repeat: round !important;
        /* background-image: var(--backgroundsrc); */
        /* border-image-source: url("/src/lib/assets/avocado-slice-background.png");
        border-image-slice: 48 68 78 68 fill;
        border-image-width: 24px 34px 39px 34px;
        border-image-outset: 0px 0px 0px 0px;
        border-image-repeat: round round; 
        border-style: solid;
        border-width: 24px 34px 39px 34px; */
        /* aspect-ratio: 2 / 1; */
    }
    .divider {
        min-width: var(--divider-size) !important;
        flex-shrink: 1;
        background-color: black;
    }
    .square, .rect, .full {
        border: solid pink var(--border-size);
        background-color: rgb(133, 133, 133);
        margin: 0;
        padding: 0;
        box-sizing:border-box;
    }
    .square > div {
        min-height: 100% !important;
        aspect-ratio: 1 / 1 !important;
    }
    .rect > div {
        min-height: 100% !important;
        aspect-ratio: 2 / 1 !important;
    }
    .two-row-space, .three-row-space {
        background-color: aquamarine;
        width: 100%;
        height: fit-content;
    }
    .two-row-space > div {
        height: 50%;
    }
    .three-row-space > div {
        width: 100%;
        height: 33%;
    }
</style>