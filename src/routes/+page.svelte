<script lang="ts">
	import TutorialPage from './components/TutorialPage.svelte';

	let currentPage = $state('home');

	const foodMap: { [key: string]: string } = {
		mixedGreens: 'Mixed Greens',
		wontonBase: 'Wonton Crisps',
		whiteRice: 'White Rice',
		brownRice: 'Brown Rice',
		spiraledCucumber: 'Spiraled Cucmber',
		quinoa: 'Quinoa',
		avacado: 'Avacado',
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
		'Add-ons': ['avacado', 'carrots', 'cucumber', 'edamame', 'krab', 'spicyKrab', 'whiteOnion'],
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
		'Toppings B': [
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
- Inside, put 1 scoop of Ahi Tuna, 3 slices of avacado, sesame seeds, green onions, crunchy onions. \n\
- Add hot blue poke and chipotle Mayo.',
			foods: {
				ahiTuna: '1 scoop',
				avacado: '3 slices',
				sesameSeeds: true,
				greenOnion: true,
				crunchyOnions: true,
				hot: true,
				chipotle: true
			}
		}
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
		avacado: '',
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
			isSelected[proteinA] = proteinCounts[proteinA]
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
			isSelected[proteinB] = proteinCounts[proteinB]
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
			isSelected[proteinC] = proteinCounts[proteinC]
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
		avacado: 0,
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
	};

	const goToTutorialPage = () => {
		currentPage = 'tutorial-page';
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

<div class:invisible={currentPage !== 'home'}>
	<button onclick={goToTutorialPage}>Tutorial Page</button>
	<div class="flex-container horizontal base">
		<div id="dry-toppings" class="flex-container">
			<div class="flex-container horizontal">
				<div id="wonton-crisps" class="rect" class:unselected={!isSelected.wontonCrisps}>
					<span>wonton{extraDetails.wontonCrisps}</span>
				</div>
				<div id="crunchy-onions" class="rect" class:unselected={!isSelected.crunchyOnions}>
					<span>crunchy onions{extraDetails.crunchyOnions}</span>
				</div>
			</div>
			<div class="flex-container horizontal">
				<div id="seaweed-flakes" class="rect" class:unselected={!isSelected.seaweedFlakes}>
					<span>flakes{extraDetails.seaweedFlakes}</span>
				</div>
				<div id="sesame-seeds" class="circle" class:unselected={!isSelected.sesameSeeds}>
					<span>seeds{extraDetails.sesameSeeds}</span>
				</div>
				<div id="sesame-oil" class="circle" class:unselected={!isSelected.sesameOil}>
					<span>oil{extraDetails.sesameOil}</span>
				</div>
			</div>
		</div>
		<div id="left-side-line" class="flex-container">
			<div class="flex-container horizontal">
				<div id="mango" class="square-line rect" class:unselected={!isSelected.mango}>
					<span>mango{extraDetails.mango}</span>
				</div>
				<div id="wasabi" class="square-line rect" class:unselected={!isSelected.wasabi}>
					<span>wasabi{extraDetails.wasabi}</span>
				</div>
				<div id="lime" class="square-line rect" class:unselected={!isSelected.lime}>
					<span>lime{extraDetails.lime}</span>
				</div>
				<div class="flex-container horizontal square-line rect">
					<div id="chipotle" class="sauce circle" class:unselected={!isSelected.chipotle}>
						<span>ch</span>
					</div>
				</div>
				<div class="flex-container horizontal rect-line rect">
					<div id="fire" class="sauce circle" class:unselected={!isSelected.fire}>
						<span>f bp</span>
					</div>
					<div id="hot" class="sauce circle" class:unselected={!isSelected.hot}>
						<span>h bp</span>
					</div>
					<div id="spicy" class="sauce circle" class:unselected={!isSelected.spicy}>
						<span>s bp</span>
					</div>
				</div>
				<div
					id="spicy-chicken"
					class="square-line rect"
					class:unselected={!isSelected.spicyChicken}
				>
					<span>spicy chicken{extraDetails.spicyChicken}</span>
				</div>
				<div id="chicken" class="square-line rect" class:unselected={!isSelected.chicken}>
					<span>chicken{extraDetails.chicken}</span>
				</div>
				<div class="square-line rect"><span>-</span></div>
			</div>
			<div class="flex-container horizontal">
				<div id="jalapeno" class="square-line rect" class:unselected={!isSelected.jalapeno}>
					<span>jalapenos{extraDetails.jalapeno}</span>
				</div>
				<div id="red-onion" class="square-line rect" class:unselected={!isSelected.redOnion}>
					<span>red onion{extraDetails.redOnion}</span>
				</div>
				<div id="ginger" class="square-line rect" class:unselected={!isSelected.ginger}>
					<span>ginger{extraDetails.ginger}</span>
				</div>
				<div class="flex-container horizontal square-line rect">
					<div id="sriracha" class="sauce circle" class:unselected={!isSelected.sriracha}>
						<span>sr</span>
					</div>
					<div id="eel" class="sauce circle" class:unselected={!isSelected.eel}>
						<span>eel</span>
					</div>
				</div>
				<div class="flex-container horizontal rect-line rect">
					<div id="ponzu" class="sauce circle" class:unselected={!isSelected.ponzu}>
						<span>pon</span>
					</div>
					<div id="cilantro-lime" class="sauce circle" class:unselected={!isSelected.cilantroLime}>
						<span>cil</span>
					</div>
					<div id="blue-poke" class="sauce circle" class:unselected={!isSelected.bluePoke}>
						<span>bp</span>
					</div>
				</div>
				<div id="octopus" class="square-line rect" class:unselected={!isSelected.octopus}>
					<span>octopus{extraDetails.octopus}</span>
				</div>
				<div id="yellowtail" class="square-line rect" class:unselected={!isSelected.yellowtail}>
					<span>yellowtail{extraDetails.yellowtail}</span>
				</div>
				<div id="shrimp" class="square-line rect" class:unselected={!isSelected.shrimp}>
					<span>shrimp{extraDetails.shrimp}</span>
				</div>
			</div>
			<div class="flex-container horizontal">
				<div id="cilantro" class="square-line rect" class:unselected={!isSelected.cilantro}>
					<span>cilantro{extraDetails.cilantro}</span>
				</div>
				<div id="green-onion" class="rect-line rect" class:unselected={!isSelected.greenOnion}>
					<span>green onion{extraDetails.greenOnion}</span>
				</div>
				<div id="masago" class="square-line rect" class:unselected={!isSelected.masago}>
					<span>masago{extraDetails.masago}</span>
				</div>
				<div id="seaweed-salad" class="rect-line rect" class:unselected={!isSelected.seaweedSalad}>
					<span>seaweed salad{extraDetails.seaweedSalad}</span>
				</div>
				<div
					id="grilled-salmon"
					class="square-line rect"
					class:unselected={!isSelected.grilledSalmon}
				>
					<span>grilled salmon{extraDetails.grilledSalmon}</span>
				</div>
				<div id="spicy-tuna" class="rect-line rect" class:unselected={!isSelected.spicyTuna}>
					<span>spicy tuna{extraDetails.spicyTuna}</span>
				</div>
			</div>
		</div>
		<div id="right-side-line" class="flex-container">
			<div class="flex-container horizontal">
				<div id="albacore" class="square-line rect" class:unselected={!isSelected.albacore}>
					<span>albacore{extraDetails.albacore}</span>
				</div>
				<div id="scallops" class="square-line rect" class:unselected={!isSelected.scallops}>
					<span>scallops{extraDetails.scallops}</span>
				</div>
				<div id="tofu" class="square-line rect" class:unselected={!isSelected.tofu}>
					<span>tofu{extraDetails.tofu}</span>
				</div>
				<div id="carrots" class="rect-line rect" class:unselected={!isSelected.carrots}>
					<span>carrots{extraDetails.carrots}</span>
				</div>
				<div id="white-onion" class="rect-line rect" class:unselected={!isSelected.whiteOnion}>
					<span>white onion{extraDetails.whiteOnion}</span>
				</div>
				<div
					id="spiraled-cucumber"
					class="rect-line rect"
					class:unselected={!isSelected.spiraledCucumber}
				>
					<span>spiraled cucumber{extraDetails.spiraledCucumber}</span>
				</div>
			</div>
			<div class="flex-container horizontal">
				<div id="ahi-tuna" class="rect-line rect" class:unselected={!isSelected.ahiTuna}>
					<span>ahi tuna{extraDetails.ahiTuna}</span>
				</div>
				<div class="square-line rect"><span>-</span></div>
				<div id="cucumber" class="rect-line rect" class:unselected={!isSelected.cucumber}>
					<span>p. cucumber{extraDetails.cucumber}</span>
				</div>
				<div id="edamame" class="rect-line rect" class:unselected={!isSelected.edamame}>
					<span>edamame{extraDetails.edamame}</span>
				</div>
				<div id="quinoa" class="rect-line rect" class:unselected={!isSelected.quinoa}>
					<span>quinoa{extraDetails.quinoa}</span>
				</div>
			</div>
			<div class="flex-container horizontal">
				<div id="salmon" class="rect-line rect" class:unselected={!isSelected.salmon}>
					<span>salmon{extraDetails.salmon}</span>
				</div>
				<div class="square-line rect"><span>-</span></div>
				<div id="krab" class="rect-line rect" class:unselected={!isSelected.krab}>
					<span>krab{extraDetails.krab}</span>
				</div>
				<div id="avacado" class="rect-line rect" class:unselected={!isSelected.avacado}>
					<span>avacado{extraDetails.avacado}</span>
				</div>
				<div id="spicy-krab" class="rect-line rect" class:unselected={!isSelected.spicyKrab}>
					<span>spicy krab{extraDetails.spicyKrab}</span>
				</div>
			</div>
		</div>
		<div id="bases" class="flex-container">
			<div class="flex-container horizontal">
				<div id="mixed-greens" class="rect" class:unselected={!isSelected.mixedGreens}>
					<span>mixed greens{extraDetails.mixedGreens}</span>
				</div>
				<div id="wonton-base" class="rect" class:unselected={!isSelected.wontonBase}>
					<span>wonton crisps{extraDetails.wontonBase}</span>
				</div>
			</div>
			<div class="flex-container horizontal">
				<div id="white-rice" class="rect" class:unselected={!isSelected.whiteRice}>
					<span>white rice{extraDetails.whiteRice}</span>
				</div>
				<div id="brown-rice" class="rect" class:unselected={!isSelected.brownRice}>
					<span>brown rice{extraDetails.brownRice}</span>
				</div>
			</div>
		</div>
	</div>
	<select bind:value={selectedTutorial} onchange={updateFromRecipe}>
		{#each Object.keys(presets) as recipe}
			<option value={recipe}>
				{recipe}
			</option>
		{/each}
	</select>
	{#if presets[selectedTutorial]}
		{#each presets[selectedTutorial].instructions.split('\n') as instruction}
			<p>
				{instruction}
			</p>
		{/each}
	{/if}
	<div class:invisible={selectedTutorial !== 'bowl'} class="flex-container horizontal">
		{#each Object.entries(foods) as [foodGroupName, foodGroup]}
			{#if foodGroupName === 'Proteins'}
				<div class="flex-container">
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
				<div class="flex-container">
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


<div class:invisible={currentPage !== 'tutorial-page'}>
	<TutorialPage {returnHome}></TutorialPage>
</div>

<style>
	.invisible {
		visibility: hidden;
	}

	.base,
	.base > div {
		border: 4px black solid;
	}

	.base {
		width: min-content;
	}

	#dry-toppings {
		background-color: lightgreen;
	}

	#left-side-line {
		background-color: lightblue;
	}

	.square-line {
		height: 60px;
		width: 60px;
		margin: 2px;
		background-color: gray;
	}

	.rect-line {
		height: 60px;
		width: 132px;
		margin: 2px;
		background-color: gray;
	}

	.circle {
		border-radius: 50%;
	}

	.sauce {
		aspect-ratio: 1 / 1;
		height: 30px;
		background-color: blue;
	}

	.rect {
		border-radius: 3px;
	}

	.invisible {
		display: none;
	}

	.flex-container {
		display: flex;
		flex-direction: column;
	}

	.square-line,
	.rect-line {
		justify-content: space-around;
	}

	.horizontal {
		flex-direction: row;
	}

	#grilled-salmon,
	#mixed-greens,
	#wonton-base,
	#white-rice,
	#brown-rice,
	#wonton-crisps,
	#crunchy-onions,
	#seaweed-flakes,
	#sesame-seeds,
	#sesame-oil,
	#edamame,
	#wasabi,
	#avacado,
	#cilantro,
	#cucumber,
	#spiraled-cucumber,
	#green-onion,
	#jalapeno,
	#red-onion,
	#quinoa,
	#mango,
	#carrots,
	#masago,
	#ahi-tuna,
	#octopus,
	#salmon,
	#krab,
	#spicy-chicken,
	#spicy-tuna,
	#shrimp,
	#chipotle,
	#sriracha,
	#eel,
	#ginger,
	#spicy-krab,
	#seaweed-salad,
	#lime,
	#white-onion,
	#tofu,
	#scallops,
	#chicken,
	#albacore,
	#yellowtail,
	#fire,
	#hot,
	#spicy,
	#blue-poke,
	#cilantro-lime,
	#ponzu,
	.square-line,
	.rect-line {
		justify-content: center;
		text-align: center;
		border: 4px rgb(255, 0, 238) solid;
	}

	.square-line,
	.rect-line {
		border-color: transparent;
	}

	#grilled-salmon {
		background-color: rgb(244, 216, 175);
	}

	#mixed-greens,
	#wonton-base,
	#white-rice,
	#brown-rice,
	#wonton-crisps,
	#crunchy-onions,
	#seaweed-flakes,
	#sesame-seeds,
	#sesame-oil {
		background-color: aqua;
		margin: 2px;
	}

	#mixed-greens,
	#wonton-base,
	#white-rice,
	#brown-rice,
	#crunchy-onions,
	#wonton-crisps,
	#seaweed-flakes {
		height: 90px;
		width: 90px;
	}

	#edamame,
	#wasabi {
		background-color: rgb(97, 178, 97);
	}

	#avacado,
	#cilantro {
		background-color: rgb(50, 127, 9);
	}

	#cucumber,
	#spiraled-cucumber,
	#green-onion {
		background-color: rgb(58, 132, 58);
	}

	#jalapeno {
		background-color: rgb(16, 59, 16);
		color: white;
	}

	#red-onion {
		background-color: rgb(142, 48, 83);
		color: white;
	}

	#quinoa {
		background-color: rgb(158, 147, 80);
	}

	#mango {
		background-color: rgb(208, 168, 25);
	}

	#carrots,
	#masago {
		background-color: rgb(202, 111, 0);
	}

	#ahi-tuna,
	#octopus {
		background-color: rgb(125, 51, 74);
		color: white;
	}

	#salmon {
		background-color: rgb(200, 132, 113);
	}

	#krab {
		background-color: rgb(255, 159, 193);
	}

	#spicy-chicken {
		background-color: rgb(149, 79, 24);
	}

	#spicy-tuna {
		background-color: rgb(147, 46, 46);
		color: white;
	}

	#shrimp {
		background-color: rgb(240, 150, 184);
	}

	#chipotle {
		background-color: rgb(129, 68, 21);
	}

	#sriracha {
		background-color: rgb(163, 70, 31);
	}

	#eel {
		background-color: rgb(31, 18, 18);
		color: white;
	}

	#ginger {
		background-color: rgb(168, 127, 76);
	}

	#spicy-krab {
		background-color: rgb(161, 113, 41);
	}

	#mixed-greens,
	#seaweed-salad,
	#lime {
		background-color: green;
	}

	#white-rice,
	#white-onion,
	#tofu,
	#scallops,
	#chicken {
		background-color: rgb(226, 218, 202);
	}

	#albacore,
	#yellowtail {
		background-color: rgb(191, 147, 131);
	}

	#brown-rice {
		background-color: rgb(227, 203, 153);
	}

	#wonton-crisps,
	#wonton-base {
		background-color: rgb(175, 137, 0);
	}

	#crunchy-onions {
		background-color: rgb(112, 88, 0);
	}

	#seaweed-flakes {
		background-color: rgb(5, 49, 0);
		color: white;
	}

	#sesame-seeds {
		background-color: black;
		color: white;
		height: 40px;
		width: 40px;
	}

	#sesame-oil {
		background-color: rgb(48, 34, 0);
		color: white;
		height: 32px;
		width: 32px;
	}

	.unselected {
		background-color: gray !important;
		border-color: transparent !important;
		color: transparent !important;
	}

	span {
		height: 100%;
	}
</style>
