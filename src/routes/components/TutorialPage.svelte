<script lang="ts">
	let { returnHome } = $props();
	import lineLayout from './lineLayout.json';

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
						{#if item.name && item.name !== ''}
							<div class="square">
								<div
									style={item['image-name'] && item['image-name'] !== ''
										? `background: url(/src/lib/assets/${item['image-name']}) round;`
										: `background-color: ${item['color-code']};`}
								>
									{item.name}
								</div>
							</div>
						{:else}
							<div class="square flex horizontal">
								{#each item.items as circularItem}
									<div
										class="circle"
										style={circularItem['image-name'] && circularItem['image-name'] !== ''
											? `background: url(/src/lib/assets/${circularItem['image-name']}) round;`
											: `background-color: ${circularItem['color-code']};`}
									>
										{circularItem.name}
									</div>
								{/each}
							</div>
						{/if}
					{/if}
					{#if item.len === 2}
						{#if item.name && item.name !== ''}
							<div class="rect">
								<div
									style={item['image-name'] && item['image-name'] !== ''
										? `background: url(/src/lib/assets/${item['image-name']}) round;`
										: `background-color: ${item['color-code']};`}
								>
									{item.name}
								</div>
							</div>
						{:else}
							<div class="rect flex horizontal">
								{#each item.items as circularItem}
									<div
										class="circle"
										style={circularItem['image-name'] && circularItem['image-name'] !== ''
											? `background: url(/src/lib/assets/${circularItem['image-name']}) round;`
											: `background-color: ${circularItem['color-code']};`}
									>
										{circularItem.name}
									</div>
								{/each}
							</div>
						{/if}
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
	.square, .circle {
		flex: 1;
		/* aspect-ratio: 1 / 1; */
	}
	.rect {
		flex: 2;
		padding: 0px var(--border-size) 0px var(--border-size) !important;
		background-repeat: round !important;
	}
	.divider {
		min-width: var(--divider-size) !important;
		flex-shrink: 1;
		background-color: black;
	}
    .circle,
	.square,
	.rect,
	.full {
		border: solid pink var(--border-size);
		background-color: rgb(133, 133, 133);
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		overflow-x: hidden;
		text-align: center;
	}
	.square > div,
	.rect > div {
		color: white;
		text-shadow:
			1px 1px 2px black,
			0 0 1em black,
			0 0 0.2em black;
	}
	.square > div {
		min-height: 100% !important;
		aspect-ratio: 1 / 1 !important;
	}
	.rect > div {
		min-height: 100% !important;
		aspect-ratio: 2 / 1 !important;
	}
	.two-row-space,
	.three-row-space {
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
