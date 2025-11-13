<script lang="ts">
	let currentItem = $state(1);
	let { hideLine, showLine } = $props();

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
				count: 2,
				name: 'POK? BOWL | Regular (3 scoops of protein)',
				extra: [
					'white rice',
					'avocado',
					'cucumber',
					'edamame',
					'ahi tuna',
					'salmon',
					'shrimp',
					'blue pok?',
					'cilantro lime',
					'chipotle mayo (spice level 3)',
					'sesame oil',
					'crunchy onions',
					'green onions',
					'jalapeños',
					'limes',
					'mango',
					'Fork, yeah! 100% cornstarch, no plastic here.',
					'Notes: Good evening, please give me two scoops of avocado instead of mango!'
				]
			},
			{
				count: 1,
				name: 'COCA-COLA ZERO SUGAR'
			},
			{
				count: 1,
				name: 'OMUSUBI | KRAB SALAD',
				extra: ['blue pok?', 'chipotle mayo (spicy level 3)']
			}
		]
	};

	function processTicket(ticket: TicketData) {
		let lines = ['\n'];
		ticket.items.forEach((item) => {
			let itemName = `${item.count} ${item.name}\n`;
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
</script>

<div class="flex">
	<div class="ticket">
		{#each processTicket(ticketData) as line}
			<div>
				{line}
			</div>
		{/each}
	</div>
	<div class:invisible={currentItem !== 1}>
		<div class="flex">
			<button disabled>Previous Item</button>
			<h3>POK? BOWL 1</h3>
			<button
				onclick={() => {
					currentItem = 2;
				}}>Next Item</button
			>
			<button
				onclick={() => {
					currentItem = 4;
					hideLine();
				}}>View check list</button
			>
		</div>
		<div>
			<p>Notes: Good evening, please give me two scoops of avocado instead of mango!</p>
		</div>
	</div>
	<div class:invisible={currentItem !== 2}>
		<div class="flex">
			<button
				onclick={() => {
					currentItem = 1;
				}}>Previous Item</button
			>
			<h3>POK? BOWL 2</h3>
			<button
				onclick={() => {
					currentItem = 3;
					hideLine();
				}}>Next Item</button
			>
			<button
				onclick={() => {
					currentItem = 4;
					hideLine();
				}}>View check list</button
			>
		</div>
		<div>
			<p>Notes: Good evening, please give me two scoops of avocado instead of mango!</p>
		</div>
	</div>
	<div class:invisible={currentItem !== 3}>
		<div class="flex">
			<button
				onclick={() => {
					currentItem = 2;
					showLine();
				}}>Previous Item</button
			>
			<h3>OMUSUBI | KRAB SALAD</h3>
			<button
				disabled
				onclick={() => {
					currentItem = 2;
				}}>Next Item</button
			>
			<button
				onclick={() => {
					currentItem = 4;
				}}>View check list</button
			>
		</div>
		<div>
			<ul>
				<li>Fill Omusubi triangle with white rice</li>
				<li>Add scoop of krab salad</li>
				<li>Cover with white rice</li>
				<li>Remove from Omusubi triangle</li>
				<li>Put long Omusubi seaweed around edge of Omusubi</li>
				<li>Add sesame seeds to tip of Omusubi</li>
			</ul>
		</div>
	</div>
	<div class:invisible={currentItem !== 4}>
		<div class="checklist">
			<button
				onclick={() => {
					currentItem = 3;
				}}>Previous Item</button
			>
			<h3>CHECK LIST</h3>
			<h4>Items:</h4>
			<ul>
				<li>2 POK? BOWLs</li>
				<ul>
					<li>Notes: Good evening, please give me two scoops of avocado instead of mango!</li>
				</ul>
				<li>1 COCA-COLA ZERO SUGAR</li>
				<li>1 OMUSUBI | KRAB SALAD</li>
			</ul>

			<h4>Side sauces:</h4>
			<ul>
				<li>3 chipotle mayo</li>
				<li>2 sesame oil</li>
				<li>1 blue pok?</li>
			</ul>

			<h4>Other:</h4>
			<ul>
				<li>3 napkins</li>
				<li>4 soy sauce packets</li>
				<li>2 forks</li>
			</ul>
		</div>
	</div>
</div>
<br />

<style>
	.ticket {
		font-family: monospace;
		border: black solid 1px;
		width: min-content;
	}

	.ticket > div {
		white-space-collapse: preserve-spaces;
		width: max-content;
	}

	.checklist {
		padding: 16px;
	}

	.flex {
		display: flex;
		flex-direction: row;
		margin: 8px;
	}
	button {
		padding: 2px;
		margin: 8px;
	}

	.invisible {
		visibility: hidden;
		display: none;
	}
</style>
