<script lang="ts">
	import TutorialPage from './components/TutorialPage.svelte';
	import TicketPage from './components/TicketPage.svelte';
	import lineLayout from './components/lineLayout.json';
	import TestPage from './components/TestPage.svelte';

	let currentPage = $state('home');
	let lineVisible = $state(true);

	let hideLine = function() {
		lineVisible = false;
	}

	let showLine = function() {
		lineVisible = true;
	}

	const foodMap: { [key: string]: string } = {
		mixedGreens: 'Mixed Greens',
		wontonBase: 'Wonton Crisps',
		whiteRice: 'White Rice',
		brownRice: 'Brown Rice',
		spiraledCucumber: 'Spiraled Cucmber',
		quinoa: 'Quinoa',
		avocado: 'Avocado',
		cucumber: 'Cucumber',
		carrots: 'Carrots',
		whiteOnion: 'White Onion',
		edamame: 'Edamame',
		krab: 'Krab Salad',
		spicyKrab: 'Spicy Krab Salad',
		grilledSalmon: 'Grilled Salmon',
		ahiTuna: 'Ahi Tuna',
		octopus: 'Octopus',
		salmon: 'Salmon',
		spicyChicken: 'Spicy Chicken',
		spicyTuna: 'Spicy Tuna',
		shrimp: 'Shrimp',
		tofu: 'Tofu',
		scallops: 'Scallops',
		chicken: 'Chicken',
		albacore: 'Albacore',
		yellowtail: 'Yellowtail',
		chipotle: 'Chipotle Mayo',
		sriracha: 'Sriracha Mayo',
		eel: 'Eel Sauce',
		fire: 'Fire Blue Poke',
		hot: 'Hot Blue Poke',
		spicy: 'Spicy Blue Poke',
		bluePoke: 'Blue Poke',
		cilantroLime: 'Cilantro Lime',
		ponzu: 'Ponzu',
		redOnion: 'Red Onion',
		wasabi: 'Wasabi',
		cilantro: 'Cilantro',
		greenOnion: 'Green Onion',
		jalapeno: 'Jalapeño',
		mango: 'Mango',
		masago: 'Masago',
		ginger: 'Ginger',
		seaweedSalad: 'Seaweed Salad',
		lime: 'Lime',
		wontonCrisps: 'Wonton Crisps',
		crunchyOnions: 'Crunchy Onions',
		seaweedFlakes: 'Seaweed Flakes',
		sesameSeeds: 'Sesame Seeds',
		sesameOil: 'Sesame Oil'
	};

	let foods = {
		Base: ['brownRice', 'mixedGreens', 'quinoa', 'spiraledCucumber', 'whiteRice', 'wontonBase'],
		'Add-ons': ['avocado', 'carrots', 'cucumber', 'edamame', 'krab', 'spicyKrab', 'whiteOnion'],
		Proteins: [
			'ahiTuna',
			'salmon',
			'albacore',
			'chicken',
			'grilledSalmon',
			'octopus',
			'scallops',
			'shrimp',
			'spicyChicken',
			'spicyTuna',
			'tofu',
			'yellowtail'
		],
		Sauces: [
			'bluePoke',
			'chipotle',
			'cilantroLime',
			'eel',
			'fire',
			'hot',
			'ponzu',
			'sesameOil',
			'spicy',
			'sriracha'
		],
		Toppings: [
			'cilantro',
			'ginger',
			'greenOnion',
			'jalapeno',
			'lime',
			'mango',
			'masago',
			'redOnion',
			'seaweedSalad',
			'wasabi'
		],
		'Dry Toppings': ['wontonCrisps', 'crunchyOnions', 'seaweedFlakes', 'sesameSeeds']
	};

	const presets: {
		[key: string]: { instructions: string; foods: { [food: string]: boolean | string } };
	} = {
		bowl: {
			instructions: '',
			foods: {}
		},
		'Yakitori Chicken Skewers': {
			instructions:
				'- Place 3 yakitori chicken skewers in air frier for 7 minutes.\n\
- Grab a small rectangular container and cover base with white rice. Set aside.\n\
- Place yakitori chicken skewers on rice, drizzle eel sauce, and place green onions and sesame seeds.',
			foods: {
				sesameSeeds: true,
				greenOnion: true,
				eel: true
			}
		},
		'Spicy Poké Locos': {
			instructions:
				'- Open bag of doritos. \n\
- Inside, put 1 scoop of Ahi Tuna, 3 slices of avocado, sesame seeds, green onions, crunchy onions. \n\
- Add hot blue poke and chipotle Mayo.',
			foods: {
				ahiTuna: '1 scoop',
				avocado: '3 slices',
				sesameSeeds: true,
				greenOnion: true,
				crunchyOnions: true,
				hot: true,
				chipotle: true
			}
		},
		'ticket': {
			instructions: '',
			foods: {
				whiteRice: true,
				avocado: true,
				cucumber: true,
				edamame: true,
				ahiTuna: true,
				salmon: true,
				shrimp: true,
				bluePoke: true,
				cilantroLime: true,
				crunchyOnions: true,
				greenOnion: true,
				jalapeno: true,
				lime: true,
				mango: true
			}
		},
	};

	let extraDetails: { [key: string]: string } = $state({
		grilledSalmon: '',
		mixedGreens: '',
		wontonBase: '',
		whiteRice: '',
		brownRice: '',
		wontonCrisps: '',
		crunchyOnions: '',
		seaweedFlakes: '',
		sesameSeeds: '',
		sesameOil: '',
		edamame: '',
		wasabi: '',
		avocado: '',
		cilantro: '',
		cucumber: '',
		spiraledCucumber: '',
		greenOnion: '',
		jalapeno: '',
		redOnion: '',
		quinoa: '',
		mango: '',
		carrots: '',
		masago: '',
		ahiTuna: '',
		octopus: '',
		salmon: '',
		krab: '',
		spicyChicken: '',
		spicyTuna: '',
		shrimp: '',
		chipotle: '',
		sriracha: '',
		eel: '',
		ginger: '',
		spicyKrab: '',
		seaweedSalad: '',
		lime: '',
		whiteOnion: '',
		tofu: '',
		scallops: '',
		chicken: '',
		albacore: '',
		yellowtail: '',
		fire: '',
		hot: '',
		spicy: '',
		bluePoke: '',
		cilantroLime: '',
		ponzu: ''
	});

	let proteinA = $state('');
	let proteinB = $state('');
	let proteinC = $state('');
	let proteinD = $state('');

	let newProteinA = $state('');
	let newProteinB = $state('');
	let newProteinC = $state('');
	let newProteinD = $state('');

	let proteinCounts: { [key: string]: number } = $state({
		grilledSalmon: 0,
		ahiTuna: 0,
		octopus: 0,
		salmon: 0,
		spicyChicken: 0,
		spicyTuna: 0,
		shrimp: 0,
		tofu: 0,
		scallops: 0,
		chicken: 0,
		albacore: 0,
		yellowtail: 0
	});

	const updateProteinA = () => {
		if (proteinA !== '') {
			proteinCounts[proteinA]--;
			isSelected[proteinA] = proteinCounts[proteinA];
			extraDetails[proteinA] =
				` (${proteinCounts[proteinA]} scoop${proteinCounts[proteinA] > 1 ? 's' : ''})`;
			if (proteinCounts[proteinA] === 0) {
				extraDetails[proteinA] = '';
			}
		}
		proteinA = newProteinA;
		proteinCounts[proteinA]++;
		isSelected[proteinA] = proteinCounts[proteinA];
		extraDetails[proteinA] =
			` (${proteinCounts[proteinA]} scoop${proteinCounts[proteinA] > 1 ? 's' : ''})`;
	};

	const updateProteinB = () => {
		if (proteinB !== '') {
			proteinCounts[proteinB]--;
			isSelected[proteinB] = proteinCounts[proteinB];
			extraDetails[proteinB] =
				` (${proteinCounts[proteinB]} scoop${proteinCounts[proteinB] > 1 ? 's' : ''})`;
			if (proteinCounts[proteinB] === 0) {
				extraDetails[proteinB] = '';
			}
		}
		proteinB = newProteinB;
		proteinCounts[proteinB]++;
		isSelected[proteinB] = proteinCounts[proteinB];
		extraDetails[proteinB] =
			` (${proteinCounts[proteinB]} scoop${proteinCounts[proteinB] > 1 ? 's' : ''})`;
	};

	const updateProteinC = () => {
		if (proteinC !== '') {
			proteinCounts[proteinC]--;
			isSelected[proteinC] = proteinCounts[proteinC];
			extraDetails[proteinC] =
				` (${proteinCounts[proteinC]} scoop${proteinCounts[proteinC] > 1 ? 's' : ''})`;
			if (proteinCounts[proteinC] === 0) {
				extraDetails[proteinC] = '';
			}
		}
		proteinC = newProteinC;
		proteinCounts[proteinC]++;
		isSelected[proteinC] = proteinCounts[proteinC];
		extraDetails[proteinC] =
			` (${proteinCounts[proteinC]} scoop${proteinCounts[proteinC] > 1 ? 's' : ''})`;
	};

	const updateProteinD = () => {
		if (proteinD !== '') {
			proteinCounts[proteinD]--;
			isSelected[proteinD] = proteinCounts[proteinD];
			extraDetails[proteinD] =
				` (${proteinCounts[proteinD]} scoop${proteinCounts[proteinD] > 1 ? 's' : ''})`;
			if (proteinCounts[proteinD] === 0) {
				extraDetails[proteinD] = '';
			}
		}
		proteinD = newProteinD;
		proteinCounts[proteinD]++;
		isSelected[proteinD] = proteinCounts[proteinD];
		extraDetails[proteinD] =
			` (${proteinCounts[proteinD]} scoop${proteinCounts[proteinD] > 1 ? 's' : ''})`;
	};

	let isSelected: { [key: string]: number } = $state({
		grilledSalmon: 0,
		mixedGreens: 0,
		wontonBase: 0,
		whiteRice: 0,
		brownRice: 0,
		wontonCrisps: 0,
		crunchyOnions: 0,
		seaweedFlakes: 0,
		sesameSeeds: 0,
		sesameOil: 0,
		edamame: 0,
		wasabi: 0,
		avocado: 0,
		cilantro: 0,
		cucumber: 0,
		spiraledCucumber: 0,
		greenOnion: 0,
		jalapeno: 0,
		redOnion: 0,
		quinoa: 0,
		mango: 0,
		carrots: 0,
		masago: 0,
		ahiTuna: 0,
		octopus: 0,
		salmon: 0,
		krab: 0,
		spicyChicken: 0,
		spicyTuna: 0,
		shrimp: 0,
		chipotle: 0,
		sriracha: 0,
		eel: 0,
		ginger: 0,
		spicyKrab: 0,
		seaweedSalad: 0,
		lime: 0,
		whiteOnion: 0,
		tofu: 0,
		scallops: 0,
		chicken: 0,
		albacore: 0,
		yellowtail: 0,
		fire: 0,
		hot: 0,
		spicy: 0,
		bluePoke: 0,
		cilantroLime: 0,
		ponzu: 0
	});

	let selectedTutorial = $state('');

	const returnHome = () => {
		currentPage = 'home';
		showLine();
	};

	const goToTicketPage = () => {
		currentPage = 'ticket-page';
		selectedTutorial = "ticket";
		updateFromRecipe();
	};

	const goToTestPage = () => {
		currentPage = 'test-page';
	};

	const updateFromRecipe = () => {
		for (let key of Object.keys(isSelected)) {
			isSelected[key] = 0;
			extraDetails[key] = '';
		}
		for (let [ingredient, amount] of Object.entries(presets[selectedTutorial].foods)) {
			console.log(ingredient);
			isSelected[ingredient] = 1;
			if (typeof amount === 'string') {
				extraDetails[ingredient] = ` (${amount})`;
			}
		}
	};

	const check = (foodName: string) => {
		isSelected[foodName] = (isSelected[foodName] + 1) % 2;
	};
</script>


<div class:invisible={currentPage !== 'test-page'}>
	
</div>

<div class:invisible={(currentPage !== 'home' && currentPage !== 'ticket-page')}>
	
	<div class="full flex horizontal line" class:invisible={!lineVisible}>
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
										class="single"
										class:unselected={!isSelected[item['code-name'] ? item['code-name'] : '']}
										style={item['image-name'] && item['image-name'] !== ''
											? `background: url(assets/${item['image-name']}) round;`
											: `background-color: ${item['color-code']};`}
									>
										{item.name}{item['code-name'] ? extraDetails[item['code-name']] : ''}
									</div>
								</div>
							{:else}
								<div class="square flex horizontal circles">
									{#each item.items as circularItem}
										<div
											class:halfHeight={(item.items && item.items.length > 1) ||
												circularItem.isSauce}
											class:unselected={!isSelected[
												circularItem['code-name'] ? circularItem['code-name'] : ''
											]}
											class="circle"
											style={circularItem['image-name'] && circularItem['image-name'] !== ''
												? `background: url(assets/${circularItem['image-name']}) round;`
												: `background-color: ${circularItem['color-code']};`}
										>
											{circularItem.abbreviation}{circularItem['code-name']
												? extraDetails[circularItem['code-name']]
												: ''}
										</div>
									{/each}
								</div>
							{/if}
							<!-- <div class="square">
								<div
									class:unselected={!isSelected[item['code-name'] ? item['code-name'] : '']}
									style={item['image-name'] && item['image-name'] !== ''
										? `background: url(/src/lib/assets/${item['image-name']}) round;`
										: `background-color: ${item['color-code']};`}
								>
									{item.name}{item['code-name'] ? extraDetails[item['code-name']] : ''}
								</div>
							</div> -->
						{/if}
						{#if item.len === 2}
							{#if item.name && item.name !== ''}
								<div class="rect">
									<div
										class="single"
										class:unselected={!isSelected[item['code-name'] ? item['code-name'] : '']}
										style={item['image-name'] && item['image-name'] !== ''
											? `background: url(assets/${item['image-name']}) round;`
											: `background-color: ${item['color-code']};`}
									>
										{item.name}{item['code-name'] ? extraDetails[item['code-name']] : ''}
									</div>
								</div>
							{:else}
								<div class="rect flex horizontal circles">
									{#each item.items as circularItem}
										<div
											class:halfHeight={(item.items && item.items.length > 1) ||
												circularItem.isSauce}
											class:unselected={!isSelected[
												circularItem['code-name'] ? circularItem['code-name'] : ''
											]}
											class="circle"
											style={circularItem['image-name'] && circularItem['image-name'] !== ''
												? `background: url(assets/${circularItem['image-name']}) round;`
												: `background-color: ${circularItem['color-code']};`}
										>
											{circularItem.abbreviation}{circularItem['code-name']
												? extraDetails[circularItem['code-name']]
												: ''}
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

	<div class:invisible={currentPage !== 'home'}>

		<select bind:value={selectedTutorial} onchange={updateFromRecipe}>
			{#each Object.keys(presets) as recipe}
				<option value={recipe}>
					{recipe}
				</option>
			{/each}
		</select>
		<button onclick={goToTicketPage}>Ticket Page</button>
		<button onclick={goToTestPage}>Test Page</button>
		{#if presets[selectedTutorial]}
			{#each presets[selectedTutorial].instructions.split('\n') as instruction}
				<p>
					{instruction}
				</p>
			{/each}
		{/if}
		<div class:invisible={selectedTutorial !== 'bowl'} class="flex horizontal">
			{#each Object.entries(foods) as [foodGroupName, foodGroup]}
				{#if foodGroupName === 'Proteins'}
					<div class="flex">
						<h3>{foodGroupName}</h3>
						<select bind:value={newProteinA} onchange={updateProteinA}>
							<option value=""></option>
							{#each foodGroup as foodName}
								<option value={foodName}>
									{foodMap[foodName]}
								</option>
							{/each}
						</select>
						<select bind:value={newProteinB} onchange={updateProteinB}>
							<option value=""></option>
							{#each foodGroup as foodName}
								<option value={foodName}>
									{foodMap[foodName]}
								</option>
							{/each}
						</select>
						<select bind:value={newProteinC} onchange={updateProteinC}>
							<option value=""></option>
							{#each foodGroup as foodName}
								<option value={foodName}>
									{foodMap[foodName]}
								</option>
							{/each}
						</select>
						<select bind:value={newProteinD} onchange={updateProteinD}>
							<option value=""></option>
							{#each foodGroup as foodName}
								<option value={foodName}>
									{foodMap[foodName]}
								</option>
							{/each}
						</select>
					</div>
				{:else}
					<div class="flex">
						<h3>{foodGroupName}</h3>
						{#each foodGroup as foodName}
							<div>
								<input
									type="checkbox"
									id={`${foodName}-select`}
									onchange={() => {
										check(foodName);
									}}
									checked={isSelected[foodName] === 1}
								/>
								<label for={`${foodName}-select`}>{foodMap[foodName]}</label>
							</div>
						{/each}
					</div>
				{/if}
			{/each}
		</div>
	</div>
</div>

<div class:invisible={currentPage !== 'test-page'}>
	<TestPage/>
</div>

<div class:invisible={currentPage !== 'ticket-page'}>
	<TicketPage {showLine} {hideLine}/>
	<button onclick={returnHome}>Return Home</button>
</div>

<div class:invisible={currentPage !== 'tutorial-page'}>
	<TutorialPage {returnHome}></TutorialPage>
</div>

<style>
	:root {
		--border-size: 2px;
		--divider-size: 4px;
		font-family: Arial, Helvetica, sans-serif;
	}
	.full {
		width: 99vw;
		height: auto;
	}
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
	.square,
	.rect,
	.full {
		border: solid pink var(--border-size);
		background-color: rgb(133, 133, 133);
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		overflow-x: hidden;
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
		background-color: aquamarine;
		width: 100%;
		height: fit-content;
	}
	.three-row-space > div {
		width: 100%;
		height: 33%;
	}

	.invisible {
		visibility: hidden;
		display: none;
	}

	.unselected {
		background-color: gray !important;
		background-image: none !important;
		border-color: transparent !important;
		color: transparent !important;
		text-shadow: none !important;
	}
</style>
