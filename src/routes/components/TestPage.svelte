<script lang="ts">
	import lineLayout from './lineLayoutTrue.json';

	type TicketData = {
		server: string;
		name: string;
		number: string;
		address1: string;
		address2: string;
		items: {
			count: number;
			name: string;
			extra?: string[];
		}[];
	};
	let ticketData: TicketData = {
		server: 'Sidney P',
		name: 'Pengu',
		number: '802-326-1953',
		address1: '1953 Tree Rd',
		address2: 'San Diego, CA, 92103',
		items: [
			{
				count: 1,
				name: 'POK? BOWL | Regular (3 scoops of protein)',
				extra: [
					'white rice',
					'cucumber',
					'salmon',
					'salmon',
					'salmon',
					'sesame seeds',
					'mango',
					'Fork, yeah! 100% cornstarch, no plastic here.',
					'extra soy sauce please'
				]
			},
			{
				count: 1,
				name: 'COCA-COLA ZERO SUGAR'
			},
			{
				count: 1,
				name: 'TACO TRIO | SALMON'
			}
		]
	};
	function processTicket(ticket: TicketData) {
		let lines = ['\n'];
		ticket.items.forEach((item) => {
			let itemName = `${item.count} ${item.name}`;
			let index = 28;
			while (itemName.charAt(index - 1) !== ' ' && index > 1 && itemName.length > index) {
				index--;
			}
			let firstLine = `  ${itemName.substring(0, index)}`;
			let remainder = itemName.substring(index);
			lines.push(firstLine);
			while (remainder.length > 0) {
				index = 26;
				while (remainder.charAt(index - 1) !== ' ' && index > 1 && remainder.length > index) {
					index--;
				}
				lines.push(`    ${remainder.substring(0, index)}\n`);
				remainder = remainder.substring(index);
			}
			if (item.extra) {
				item.extra.forEach((subItem) => {
					let name = subItem;
					while (name.length > 0) {
						index = 26;
						while (name.charAt(index - 1) !== ' ' && index > 1 && name.length > index) {
							index--;
						}
						lines.push(`      ${name.substring(0, index)}\n`);
						name = name.substring(index);
					}
				});
			}
			lines.push('\n');
		});
		return lines;
	}
	type ContainerType = 'metal' | 'plastic' | 'nothing';
	function setHandItem(containerType: ContainerType) {
		if (handItem === 'nothing' || containerType === 'nothing') {
			handItem = containerType;
		}
	}
	function handItemCheck(containerType: ContainerType) {
		return handItem === containerType;
	}
	type SideBar = 'ticket' | 'actionMenu';
	function setSelectedSideBar(sideBar: SideBar) {
		if (sideBar === 'ticket') {
			previousSelected = 'ticket';
		} else {
			previousSelected = 'actionMenu';
		}
	}
	function verifySelected(sideBar: SideBar) {
		return previousSelected === sideBar;
	}

	let currentArea = $state('line');
	type Area = 'line' | 'workspace';
	function setScene(area: Area) {
		currentArea = area;
	}

	let ticketVisible = $state(false);
	let actionMenuVisible = $state(false);
	let previousSelected = $state('ticket');
	let leftHandDiv: HTMLDivElement;
	let rightHandDiv: HTMLDivElement;
	let rightHandItem: RightHandItem | null = null;
	let ticket: HTMLDivElement;
	let handItem = $state('nothing');
	let ticketWidth = $state(0);
	let sideMenuTranslationStyle = $derived(`transform: translateX(${ticketWidth}px);`);
	let menuTranslateStyle = $derived(
		ticketVisible || actionMenuVisible ? sideMenuTranslationStyle : 'transform: translateX(0px);'
	);
	let itemsInBowl: string[] = $state([]);
	type HandItems =
		| 'white-rice-cartoon'
		| 'mango-cartoon'
		| 'seeds-cartoon'
		| 'salmon-cartoon'
		| 'plastic-bowl-lid'
		| 'flakes-cartoon'
		| 'cucumber-cartoon';
	function addItemToHand(item: HandItems | string) {
		if (
			!handItemCheck('nothing') && 
			item.length > 0 &&
			(itemsInBowl.length === 0 || itemsInBowl[itemsInBowl.length - 1] !== 'plastic-bowl-lid') &&
			(item !== 'plastic-bowl-lid' || handItemCheck('plastic'))
		) {
			itemsInBowl.push(item);
		}
	}
	let isWearingGlove = $state(false);

	type Bowl = {
		type: 'plastic' | 'metal';
		contents: string[];
	};
	type SauceContainer = {
		contents: 'chipotle-mayo' | 'sriracha-mayo';
	};
	// type PrepItemLarge = 'nacho-box-large'
	// type PrepItemMedium = 'taco-box'
	// type PrepItemSmall = 'sauce-container'
	// type PrepTool = 'cutting-board' | 'taco-stand'

	const tacoLayers = [
		'taco-chipotle-line',
		'taco-chipotle-squiggle',
		'taco-coleslaw',
		'taco-green-onion',
		'taco-sesame-seeds',
		'taco-salmon',
		'taco-avocado',
		'taco-shell'
	] as const;
	type TacoLayer = (typeof tacoLayers)[number];

	const rightHandItems = ['sesame-seed-container'] as const;
	type RightHandItem = (typeof rightHandItems)[number];

	const prepItemLarge = ['nacho-box-large'] as const;
	type PrepItemLarge = (typeof prepItemLarge)[number];
	function isPrepItemLarge(s: string): s is PrepItemLarge {
		return (prepItemLarge as readonly string[]).includes(s);
	}

	const prepItemMedium = ['taco-box'] as const;
	type PrepItemMedium = (typeof prepItemMedium)[number];
	function isPrepItemMedium(s: string): s is PrepItemMedium {
		return (prepItemMedium as readonly string[]).includes(s);
	}

	const prepItemSmall = ['sauce-container'] as const;
	type PrepItemSmall = (typeof prepItemSmall)[number];
	function isPrepItemSmall(s: string): s is PrepItemSmall {
		return (prepItemSmall as readonly string[]).includes(s);
	}

	const prepTools = ['cutting-board', 'taco-stand'] as const;
	type PrepTool = (typeof prepTools)[number];
	function isPrepTool(s: string): s is PrepTool {
		return (prepTools as readonly string[]).includes(s);
	}

	type BaggingTableItem = Bowl;
	let baggingTableItems: BaggingTableItem[] = $state([]);

	type PrepItems = {
		large: PrepItemLarge[];
		medium: (Bowl | PrepItemMedium)[];
		small: (SauceContainer | PrepItemSmall)[];
		tools: PrepTool[];
	};
	let prepTableItems: PrepItems = $state({
		large: [],
		medium: [],
		small: [],
		tools: []
	});
	function addItemToPrepTable(item: string) {
		if (isPrepTool(item)) {
			prepTableItems.tools.push(item);
		} else if (isPrepItemLarge(item)) {
			prepTableItems.large.push(item);
		} else if (isPrepItemMedium(item)) {
			prepTableItems.medium.push(item);
		} else if (isPrepItemSmall(item)) {
			prepTableItems.small.push(item);
		}
		// if (typeof item === 'string') {
		// 	if (item.length > 0) {
		// 		console.log(item);
		// 		itemsOnPrepTable.push(item);
		// 	}
		// } else {
		// 	let bowl: string[] = [];
		// 	item.forEach((itemString) => {
		// 		bowl.push(itemString);
		// 	});
		// 	prepTableBowls.push(bowl);
		// }
	}

	let actions: {
		[key: string]: {
			actionAppearanceCondition?: () => boolean;
			action: string;
			function?: () => void;
		}[];
	} = {
		line: [
			{
				action: 'change gloves',
				function: putOnGloves
			},
			{
				action: "throw away bowl's contents",
				function: throwAwayBowl
			},
			{
				action: 'remove bowl',
				function: removeBowlFromHand
			},
			{
				action: 'staple bag & place in fridge'
			},
			{
				action: 'get full order nacho container'
			},
			{
				action: 'fill cup with ice'
			},
			{
				action: 'fill cup with Hibiscus'
			},
			{
				action: 'fill cup with Black Tea'
			},
			{
				action: 'fill cup with Lemonade'
			},
			{
				action: 'grab drink from fridge'
			}
		],
		prepArea: [
			{
				action: 'change gloves',
				function: putOnGloves
			},
			{
				action: 'get cutting board',
				function: () => addItemToPrepTable('cutting-board')
			},
			{
				action: 'get taco stand',
				function: () => addItemToPrepTable('taco-stand')
			},
			{
				action: 'get taco shells',
				function: () => addItemToPrepTable('taco-box')
			},
			{
				action: 'get tin foil'
			},
			{
				action: 'get wax paper (small)'
			},
			{
				action: 'get wax paper (large)'
			},
			{
				action: 'cover with siran wrap'
			},
			{
				action: 'get pokerrito seaweed'
			},
			{
				action: 'get long Omusubi seaweed'
			},
			{
				action: 'get short Omusubi seaweed'
			},
			{
				action: 'get spam musubi seaweed'
			}
		],
		metalBowl: [
			{ action: 'mix contents' },
			{ action: 'throw away contents', function: throwAwayBowl },
			{ action: 'remove (put bowl in sink)', function: removeBowlFromHand },
			{ action: 'put bowl in prep area', function: placeBowlInPrepArea }
		],
		plasticBowl: [
			{ action: 'throw away contents', function: throwAwayBowl },
			{
				action: 'throw away bowl',
				function: removeBowlFromHand,
			},
			{
				action: 'cover with lid',
				function: () => {
					addItemToHand('plastic-bowl-lid');
				},
				actionAppearanceCondition: () => {
					return (
						itemsInBowl.length > 0 && itemsInBowl[itemsInBowl.length - 1] !== 'plastic-bowl-lid'
					);
				}
			},
			{
				action: 'remove lid',
				function: () => {
					itemsInBowl.pop();
				},
				actionAppearanceCondition: () => {
					return (
						itemsInBowl.length > 0 && itemsInBowl[itemsInBowl.length - 1] === 'plastic-bowl-lid'
					);
				}
			},
			{ action: 'put bowl in bagging area', function: placeBowlInBaggingArea }
		],
		tacobox: [
			{ 
				action: 'place taco shells in taco shell holder', 
				function: () => {},// {placeItemOnTacoHolder('taco-shell')},
				actionAppearanceCondition: () => {
					return prepTableItems.tools.some(it => it === "taco-stand")
				}
			}
		]
	};

	const viewTicket = () => {
		ticketWidth = ticket.clientWidth;
		ticketVisible = true;
		setSelectedSideBar('ticket');
	};

	const hideTicket = () => {
		ticketVisible = false;
	};

	const viewActions = () => {
		ticketWidth = ticket.clientWidth;
		actionMenuVisible = true;
		setSelectedSideBar('actionMenu');
	};

	const hideActions = () => {
		actionMenuVisible = false;
	};

	function putOnGloves() {
		isWearingGlove = true;
	}

	function throwAwayBowl() {
		itemsInBowl = [];
	}

	function removeBowlFromHand() {
		throwAwayBowl();
		setHandItem('nothing');
	}

	function placeBowlInPrepArea() {
		prepTableItems.medium.push({ type: 'metal', contents: itemsInBowl.slice() });
		removeBowlFromHand();
	}

	function placeBowlInBaggingArea() {
		baggingTableItems.push({ type: 'plastic', contents: itemsInBowl.slice() });
		removeBowlFromHand();
	}
</script>

<div
	class="floor"
	class:gloved-cursor={isWearingGlove}
	class:cursor={!isWearingGlove}
	class:workspace-floor={currentArea === 'workspace'}
>
	<div class="floorshading"></div>
	<div class="ticket-container">
		<div
			class="ticket"
			role="presentation"
			bind:this={ticket}
			class:selected-tab={ticketVisible || verifySelected('ticket')}
			style={menuTranslateStyle}
			onmouseenter={viewTicket}
			onmouseleave={hideTicket}
		>
			{#each processTicket(ticketData) as line}
				<div>
					{line}
				</div>
			{/each}
		</div>
		<div
			class="ticket-hover"
			role="presentation"
			style={menuTranslateStyle}
			onmouseenter={viewTicket}
			onmouseleave={hideTicket}
		>
			View ticket
		</div>
	</div>
	<div class="action-container">
		<div
			class="action-menu flex"
			role="presentation"
			class:selected-tab={actionMenuVisible || verifySelected('actionMenu')}
			style={`${menuTranslateStyle}; width: ${ticketWidth}px;`}
			onmouseenter={viewActions}
			onmouseleave={hideActions}
		>
			{#each currentArea === 'line' ? actions.line : actions.prepArea as action}
				<button
					onclick={action.function}>{action.action}</button
				>
			{/each}
		</div>
		<div
			class="action-hover"
			role="presentation"
			style={menuTranslateStyle}
			onmouseenter={viewActions}
			onmouseleave={hideActions}
		>
			View Actions
		</div>
	</div>
	<div class:invisible={currentArea !== 'line'}>
		<div class="metal-table">
			<div class="line-container">
				<div class="full flex horizontal line">
					<div class="flex three-row-space">
						{#each lineLayout as lineRow}
							<div class="flex horizontal line">
								{#each lineRow as item}
									{#if item.len === 1}
										{#if item.name && item.name !== ''}
											<div class="square">
												<div
													class="single"
													style={item['image-name'] && item['image-name'] !== ''
														? `background: url(assets/${item['image-name']}) round;`
														: `background-color: ${item['color-code']};`}
													role="presentation"
													onclick={() => addItemToHand(item['class-name'] || '')}
												>
													{item.name}
												</div>
											</div>
										{:else}
											<div class="square flex horizontal circles">
												{#each item.items as circularItem}
													<div
														class:halfHeight={(item.items && item.items.length > 1) ||
															circularItem.isSauce}
														class="circle"
														style={circularItem['image-name'] && circularItem['image-name'] !== ''
															? `background: url(assets/${circularItem['image-name']}) round;`
															: `background-color: ${circularItem['color-code']};`}
														role="presentation"
														onclick={() => addItemToHand(circularItem['class-name'] || '')}
													>
														{circularItem.abbreviation}
													</div>
												{/each}
											</div>
										{/if}
									{/if}
									{#if item.len === 2}
										{#if item.name && item.name !== ''}
											<div class="rect">
												<div
													class="single"
													style={item['image-name'] && item['image-name'] !== ''
														? `background: url(assets/${item['image-name']}) round;`
														: `background-color: ${item['color-code']};`}
													role="presentation"
													onclick={() => addItemToHand(item['class-name'] || '')}
												>
													{item.name}
												</div>
											</div>
										{:else}
											<div class="rect flex horizontal circles">
												{#each item.items as circularItem}
													<div
														class:halfHeight={(item.items && item.items.length > 1) ||
															circularItem.isSauce}
														class="circle"
														style={circularItem['image-name'] && circularItem['image-name'] !== ''
															? `background: url(assets/${circularItem['image-name']}) round;`
															: `background-color: ${circularItem['color-code']};`}
														role="presentation"
														onclick={() => addItemToHand(circularItem['class-name'] || '')}
													>
														{circularItem.abbreviation}
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
			</div>
		</div>
		<div class="bagging-table flex horizontal">
			<div class="napkins"></div>
			<div class="soy-container"></div>
			<div class="small-bag"></div>
			<div class="large-bag"></div>
			{#each baggingTableItems as bowl}
				<div class="plastic-bowl">
					{#each bowl.contents as item}
						<span class={item}></span>
					{/each}
				</div>
			{/each}
		</div>
		<div class="rice-table flex">
			<div class="flex horizontal bowl-container">
				<div
					class="plastic-bowl-rice-table"
					role="presentation"
					onclick={() => setHandItem('plastic')}
				></div>
				<div
					class="metal-bowl-rice-table"
					role="presentation"
					onclick={() => setHandItem('metal')}
				></div>
			</div>
			<div
				class="white-rice"
				role="presentation"
				onclick={() => addItemToHand('white-rice-cartoon')}
			>
				<p>white rice</p>
			</div>
			<div class="brown-rice"><p>brown rice</p></div>
		</div>
		<div class="items-on-line flex">
			<div
				class="left-hand"
				bind:this={leftHandDiv}
				class:metal-bowl={handItemCheck('metal')}
				class:plastic-bowl={handItemCheck('plastic')}
			>
				{#each itemsInBowl as item}
					<span class={item}></span>
				{/each}
				{#if handItemCheck('metal')}
					<div class="item-action">
						{#each actions.metalBowl as metalBowlAction}
							<button onclick={metalBowlAction.function}>{metalBowlAction.action}</button>
						{/each}
					</div>
				{:else if handItemCheck('plastic')}
					<div class="item-action">
						{#each actions.plasticBowl as plasticBowlAction}
							{#if !plasticBowlAction.actionAppearanceCondition || plasticBowlAction.actionAppearanceCondition()}
								<button onclick={plasticBowlAction.function}>{plasticBowlAction.action}</button>
							{/if}
						{/each}
					</div>
				{/if}
			</div>
			<div class="right-hand" bind:this={rightHandDiv}>
				{#if rightHandItem !== null}
					<div class={rightHandItem}></div>
				{/if}
			</div>
		</div>
		<button
			class="go-to-workspace"
			onclick={() => {
				setScene('workspace');
			}}>Go to prep counter</button
		>
	</div>
	<div class:invisible={currentArea !== 'workspace'} class="prep-table">
		<div class="prep-table-item-area">
			<div class="large-height-prep-container">
				<div class="large-height-prep">
					<span class="empty-left-prep-space"></span>
					{#each prepTableItems.large as largeItem}{/each}
				</div>
				<div class="medium-height-prep-container">
					<div class="medium-height-prep">
						{#each prepTableItems.medium as mediumItem}
							{#if mediumItem === 'taco-box'}
								<div class="taco-box">
									<div class="item-action">
										{#each actions.tacobox as tacoboxAction}
											{#if !tacoboxAction.actionAppearanceCondition || tacoboxAction.actionAppearanceCondition()}
												<button onclick={tacoboxAction.function}>{tacoboxAction.action}</button>
											{/if}
										{/each}
									</div>
								</div>
							{:else}
								<div class="metal-bowl">
									{#each mediumItem.contents as item}
										<span class={item}></span>
									{/each}
								</div>
							{/if}
						{/each}
					</div>
					<div class="medium-height-prep">
						<!-- {#each prepTableItems.medium as mediumItem}{/each} -->
					</div>
				</div>
			</div>
			<div class="small-height-prep">
				<span class="empty-left-prep-space"></span>
				{#each prepTableItems.small as smallItem}{/each}
			</div>
		</div>
		<div class="prep-table-prep-area">
			{#each prepTableItems.tools as prepTool}
				<div class={prepTool}></div>
			{/each}
			<!-- <div class="taco-stand">
				<div class="taco-shell"></div>
				<div class="taco-chipotle-line"></div>
				<div class="taco-coleslaw"></div>
				<div class="taco-salmon"></div>
				<div class="taco-green-onion"></div>
				<div class="taco-sesame-seeds"></div>
				<div class="taco-avocado"></div>
				<div class="taco-sesame-seeds"></div>
				<div class="taco-chipotle-squiggle"></div>
			</div> -->
		</div>
		<button
			class="go-to-line"
			onclick={() => {
				setScene('line');
			}}>Go to Line</button
		>
	</div>
</div>

<style>
	.item-action {
		position: absolute;
		min-width: 10%;
		min-height: 10%;
		left: 90%;
		top: 0;
		z-index: 99;
		background-image: url('$img/napkins.png');
		background-size: 100% auto;
	}

	.item-action:hover {
		background: none;
	}

	.item-action:hover button {
		display: block !important;
	}

	.item-action button {
		width: max-content;
		display: none;
	}

	.empty-left-prep-space {
		width: 120px;
	}
	.large-height-prep-container {
		display: flex;
		flex-direction: row;
		height: 75%;
		width: fit-content;
		/* background-color: blue; */
	}

	.large-height-prep {
		display: flex;
		flex-direction: row;
		height: 100%;
		width: fit-content;
		/* background-color: pink; */
	}

	.medium-height-prep-container {
		height: 100%;
	}

	.medium-height-prep {
		display: flex;
		flex-direction: row;
		height: 50%;
		/* background-color: purple; */
	}

	.small-height-prep {
		display: flex;
		flex-direction: row;
		height: 20%;
		width: fit-content;
		/* background-color: green; */
	}

	.prep-table {
		height: 85vh;
		width: 100vw;
		padding-bottom: 30px;
		background: url('$img/long table down.png') repeat-x;
		background-size: auto 100%;
	}

	.prep-table-prep-area > div {
		max-width: 40vw;
	}

	.prep-table-prep-area,
	.prep-table-item-area {
		height: 85vh;
		position: absolute;
		display: flex;
		flex-direction: column;
	}

	.prep-table-item-area {
		/* padding-left: 140px; */
		left: 0;
		max-width: 60vw;
		overflow-x: auto;
	}

	.prep-table-prep-area {
		right: 0;
		width: 40vw;
		overflow-y: auto;
		display: flex;
	}

	.cutting-board {
		height: auto;
		/* min-height: 33%; */
		aspect-ratio: 40 / 33;
		margin: 0px 8px 0px 8px;
		background: url('$img/cutting board.png') no-repeat;
		background-size: auto 100%;
	}

	.taco-box {
		height: auto;
		aspect-ratio: 19 / 15;
		margin: 16px;
		background: url('$img/tacobox.png') no-repeat;
		background-size: 100% 100%;
	}

	.taco-stand {
		height: auto;
		aspect-ratio: 23 / 15;
		max-width: 25vw !important;
		background: url('$img/taco stand.png') no-repeat;
		background-size: 100% 100%;
	}

	.taco-stand > div {
		background-size: 100% auto;
		position: absolute;
		top: 0;
		left: 0;
		aspect-ratio: 23 / 15;
		max-width: 25vw !important;
		z-index: 5 !important;
	}

	.taco-shell {
		height: 100%;
		width: 100%;
		position: absolute;
		background: url('$img/tacos.png') no-repeat;
		background-size: 100% 100%;
	}

	.taco-chipotle-line {
		height: 100%;
		width: 100%;
		position: absolute;
		top: 0;
		background: url('$img/taco chipotle line.png') no-repeat;
		background-size: 100% 100%;
	}
	.taco-chipotle-squiggle {
		height: 100%;
		width: 100%;
		position: relative;
		background: url('$img/taco chipotle squiggle.png') no-repeat;
		background-size: 100% 100%;
	}
	.taco-coleslaw {
		height: 100%;
		width: 100%;
		position: absolute;
		background: url('$img/taco coleslaw.png') no-repeat;
		background-size: 100% 100%;
	}
	.taco-green-onion {
		height: 100%;
		width: 100%;
		position: absolute;
		background: url('$img/taco green onion.png') no-repeat;
		background-size: 100% 100%;
	}
	.taco-sesame-seeds {
		height: 100%;
		width: 100%;
		position: absolute;
		background: url('$img/taco sesame seeds.png') no-repeat;
		background-size: 100% 100%;
	}
	.taco-salmon {
		height: 100%;
		width: 100%;
		position: absolute;
		background: url('$img/taco salmon.png') no-repeat;
		background-size: 100% 100%;
	}
	.taco-avocado {
		height: 100%;
		width: 100%;
		position: absolute;
		background: url('$img/taco avocado.png') no-repeat;
		background-size: 100% 100%;
	}

	.invisible {
		display: none;
		visibility: hidden;
	}

	.go-to-workspace {
		position: absolute;
		bottom: 0;
		right: 0;
	}

	.go-to-line {
		position: absolute;
		left: 0;
		bottom: 0;
	}

	button {
		padding: 8px;
		margin: 8px;
		font: large;
	}

	.cursor,
	.cursor button {
		cursor: url('$img/gloveless cursor.png'), auto;
	}

	.gloved-cursor,
	.gloved-cursor button {
		cursor: url('$img/gloved cursor.png'), auto;
	}

	.white-rice-cartoon {
		background: url('$img/rice cartoon.png') no-repeat;
	}
	.mango-cartoon {
		background: url('$img/mango cartoon.png') no-repeat;
	}
	.seeds-cartoon {
		background: url('$img/seeds cartoon.png') no-repeat;
	}
	.salmon-cartoon {
		background: url('$img/salmon cartoon.png') no-repeat;
	}
	.flakes-cartoon {
		background: url('$img/flakes cartoon.png') no-repeat;
	}
	.cucumber-cartoon {
		background: url('$img/cucumber cartoon.png') no-repeat;
	}
	.plastic-bowl-lid {
		background: url('$img/plastic bowl lid.png') no-repeat;
	}

	.white-rice-cartoon,
	.mango-cartoon,
	.seeds-cartoon,
	.plastic-bowl-lid,
	.salmon-cartoon,
	.flakes-cartoon,
	.cucumber-cartoon {
		background-size: 100% 100%;
		height: 100%;
		width: 100%;
		left: 0;
		position: absolute;
		top: 0;
		z-index: 6 !important;
	}

	.metal-bowl {
		position: relative;
		height: 100%;
		aspect-ratio: 1;
		background: url('$img/metal bowl small.png') no-repeat;
		background-size: 100% auto;
	}

	.plastic-bowl {
		position: relative;
		height: 100%;
		aspect-ratio: 1;
		background: url('$img/plastic bowl small.png') no-repeat;
		background-size: 100% auto;
		z-index: 4;
	}

	.left-hand {
		position: absolute;
		height: min(50vh, 35vw);
		width: min(50vh, 35vw);
		top: 30vh;
		right: 50vw;
		z-index: 4;
		overflow: visible;
	}

	.right-hand {
		position: absolute;
		height: min(50vh, 50vw);
		width: min(50vh, 50vw);
		top: 30vh;
		left: 50vw;
		z-index: 3;
		overflow: visible;
	}

	.selected-tab {
		z-index: 100 !important;
	}

	.soy-container {
		height: 8vh;
		width: auto; /* 24 * 19/33 */
		z-index: 1;
		aspect-ratio: 1 / 1;
		background: url('$img/soy container small.png');
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	.small-bag {
		/* image ratio: 19:33 */
		height: 24vh;
		width: auto; /* 24 * 19/33 */
		z-index: 1;
		aspect-ratio: 19 / 33;
		background: url('$img/small bag small.png');
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	.large-bag {
		/* image ratio: 19:33 */
		height: 24vh;
		width: auto; /* 24 * 19/33 */
		z-index: 1;
		aspect-ratio: 19 / 33;
		background: url('$img/large bag small.png');
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	.floorshading {
		position: absolute;
		top: 0;
		left: 0;
		z-index: -1;
		height: 100vh;
		width: 100vw;
		background: url('$img/floor light.png');
		opacity: 60%;
		background-size: 100vw 100vh;
	}

	.napkins {
		height: 24vh;
		width: auto;
		z-index: 1;
		aspect-ratio: 7 / 4;
		background: url('$img/napkins small.png');
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	.ticket-container {
		position: absolute;
	}
	
	.action-container {
		position: absolute;
	}
	
	.ticket-hover {
		position: absolute;
		height: fit-content;
		width: fit-content;
		padding: 8px;
		top: 40vh;
		height: 5vh;
		width: 10vh;
		z-index: 101;
		text-align: center;
		background-color: white;
		border: thick black solid;
		border-left: none;
		transition: transform 0.3s ease-out;
	}

	.action-hover {
		position: absolute;
		height: fit-content;
		width: fit-content;
		padding: 8px;
		z-index: 101;
		top: 49vh;
		height: 5vh;
		width: 10vh;
		text-align: center;
		background-color: rgb(142, 142, 219);
		border: thick black solid;
		border-left: none;
		transition: transform 0.3s ease-out;
	}

	.ticket {
		position: absolute;
		height: 100vh;
		border-right: thick black solid;
		z-index: 99;
		width: max-content;
		background-color: white;
		overflow-y: auto;
		right: 0;
		z-index: 2;
		padding: 0px;
		transition: transform 0.3s ease-out;
		/* transform: translateX(-100%); */
	}

	.action-menu {
		position: absolute;
		height: 100vh;
		padding: 0px;
		border-right: thick black solid;
		z-index: 99;
		padding: 0px;
		background-color: rgb(142, 142, 219);
		overflow-y: auto;
		overflow-x: hidden;
		align-items: center;
		right: 0;
		z-index: 2;
		transition: transform 0.3s ease-out;
		/* transform: translateX(-100%); */
		/* transform: translateX(100%) !important; */
	}

	.action-menu > button,
	.ticket > div {
		margin: 0px 20px 0px 8px;
	}

	.ticket > div {
		white-space-collapse: preserve;
		width: max-content;
		font-family: monaco;
	}

	.floor {
		z-index: -1;
		min-height: 100vh;
		min-width: 100vw;
		padding: 0;
		margin: 0;
		position: absolute;
		background: url('$img/tile small.png');
		background-size: 120px 120px;
		overflow: hidden;
	}

	.workspace-floor {
		background-size: 280px 280px !important;
	}

	.metal-table {
		height: 35vh;
		width: 100vw;
		position: absolute;
		flex-direction: row-reverse;
		display: flex;
		top: 0;
		background: url('/assets/long table down small.png');
		background-size: auto 100%;
		box-shadow: 3px 5px 5px black;
	}

	.bagging-table {
		height: 25vh;
		width: 70vw;
		bottom: 0;
		position: absolute;
		background: url('$img/long table up small.png');
		background-size: auto 100%;
		box-shadow: 3px -1px 5px black;
		align-items: center;
		overflow: auto hidden;
	}

	.rice-table {
		position: absolute;
		right: 0;
		top: 30vh;
		background: url('$img/long table left small.png') 0 0 repeat;
		width: 15vw;
		height: 45vh;
		align-items: center;
		background-size: 100% auto;
		box-shadow:
			-5px 5px 5px black,
			1px -1px 5px black;
	}

	.bowl-container {
		height: 28%;
		width: 100%;
		align-content: space-evenly;
	}

	.metal-bowl-rice-table {
		aspect-ratio: 1 / 1;
		max-width: 50%;
		width: auto;
		height: 100%;
		background: url('$img/metal bowl small.png') no-repeat;
		background-size: 100% auto;
	}

	.plastic-bowl-rice-table {
		aspect-ratio: 1 / 1;
		width: auto;
		max-width: 50%;
		height: 100%;
		background: url('$img/plastic bowl small.png') no-repeat;
		background-size: 100% auto;
	}

	.white-rice,
	.brown-rice {
		aspect-ratio: 1 / 1;
		height: 36%;
		max-width: 100%;
		width: auto;
		border-radius: 50%;
		align-items: center;
		text-align: center;
	}

	.white-rice {
		background-color: aqua;
	}

	.brown-rice {
		background-color: greenyellow;
	}

	.full {
		/* width: 100vw; */
		height: 100%;
		overflow-x: auto !important;
	}

	.flex {
		display: flex;
		flex-direction: column;
	}

	.line-container {
		width: fit-content;
		max-width: 100%;
		overflow-x: hidden;
	}

	.line-container {
		height: 75%;
	}

	.line {
		justify-content: space-between;
	}
	.horizontal {
		flex-direction: row;
	}
	.circles {
		justify-content: space-evenly;
	}
	.square > .halfHeight {
		max-height: 45%;
	}
	.rect > .halfHeight {
		max-height: 63%;
	}
	.rect > .circle,
	.square > .circle {
		border-radius: 50%;
		flex-shrink: 1;
		border: 1px pink solid;
		aspect-ratio: 1 / 1;
		color: white;
		justify-content: center;
		text-align: center;
		text-shadow:
			1px 1px 2px black,
			0 0 1em black,
			0 0 0.2em black;
	}
	.square {
		flex: 1;
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
	.square,
	.rect,
	.line-container {
		border: solid #00000022 var(--border-size);
		overflow-x: hidden;
	}
	.square,
	.rect,
	.full,
	.line-container {
		background-color: transparent;
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		overflow-y: hidden;
	}
	.square > .single,
	.rect > .single {
		color: white;
		justify-content: center;
		text-align: center;
		text-shadow:
			1px 1px 2px black,
			0 0 1em black,
			0 0 0.2em black;
	}
	.square > .single {
		min-height: 100% !important;
		aspect-ratio: 1 / 1 !important;
	}
	.rect > .single {
		min-height: 100% !important;
		aspect-ratio: 2 / 1 !important;
	}
	.three-row-space {
		background-color: transparent;
		width: auto;
		height: 100%;
	}
	.three-row-space > div {
		width: auto;
		height: 33.33%;
	}
	.two-row-space {
		background-color: transparent;
		width: auto;
		height: 100%;
	}
	.two-row-space > div {
		width: auto;
		height: 33.33%;
	}
</style>
