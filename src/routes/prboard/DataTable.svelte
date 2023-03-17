<script context="module" lang="ts">

	import DataTable, { Head, Body, Row, Cell, Label, SortValue } from '@smui/data-table';
	import IconButton from '@smui/icon-button';

	export type PRReps = {
		1: number;
		2: number;
		3: number;
		5: number;
		10: number;
	};

	export type PRExercise = {
		id: number;
		name: string;
		repScheme: PRReps;
	};

	export let options: PRExercise[] = [
		{
			id: 1,
			name: 'Deadlift',
			repScheme: { 1: 360, 2: 330, 3: 300, 5: 270, 10: 240 }
		},
		{
			id: 2,
			name: 'Bench Press',
			repScheme: { 1: 260, 2: 230, 3: 200, 5: 170, 10: 140 }
		},
		{
			id: 3,
			name: 'Clean',
			repScheme: { 1: 260, 2: 230, 3: 200, 5: 170, 10: 140 }
		},
		{
			id: 4,
			name: 'Snatch',
			repScheme: { 1: 180, 2: 160, 3: 1500, 5: 140, 10: 130 }
		},
		{
			id: 5,
			name: 'Back Squat',
			repScheme: { 1: 360, 2: 330, 3: 300, 5: 270, 10: 240 }
		}
	];
	let sort: keyof PRExercise;
	let sortDirection: Lowercase<keyof typeof SortValue> ;

	// handleSort function is called when the user clicks on a column header
	function handleSort(event: CustomEvent<{ columnId: keyof PRExercise }>) {
			sortDirection = sortDirection === 'ascending' ? 'ascending': 'descending' ;
			// sort the options array
			options = options.sort((a, b) => {
				if (sortDirection === 'ascending') {
					console.log(typeof a.name, a.name)
					return a.name - b.name;
				} else {
					console.log(typeof b.name, b.name)
					return b['name'] - a['name'];
				}
			});
			
		
	}
</script>

<DataTable
	sortable
	bind:sort
	bind:sortDirection
	on:SMUIDataTable:sorted={handleSort}
	table$aria-label="PRExercise list"
	style="width: 98.5vw;"
>
	<Head>
		<Row>
			<!--
        Note: whatever you supply to "columnId" is
        appended with "-status-label" and used as an ID
        for the hidden label that describes the sort
        status to screen readers.

        You can localize those labels with the
        "sortAscendingAriaLabel" and
        "sortDescendingAriaLabel" props on the DataTable.
	-->
			<Cell numeric columnId="id">
				<!-- For numeric columns, icon comes first. -->
				<IconButton class="material-icons">arrow_upward</IconButton>
				<Label>ID</Label>
			</Cell>
			<Cell columnId="name" style="width: 10%;">
				<Label>Exercise Name</Label>
				<!-- For non-numeric columns, icon comes second. -->
				<IconButton class="material-icons">arrow_upward</IconButton>
			</Cell>
			<Cell numeric columnId="repScheme">
				<IconButton class="material-icons">arrow_upward</IconButton>
				<Label>Rep Scheme</Label>
			</Cell>
			<!-- You can turn off sorting for a column. -->
		</Row>
	</Head>
	<Body>
		{#each options as option (option.id)}
			<Row>
				<Cell numeric>{option.id}</Cell>
				<Cell>{option.name}</Cell>
				{#each Object.entries(option.repScheme) as [key, value]}
					<Cell numeric>{key} Rep Max - {value}lbs.</Cell>
				{/each}
			</Row>
		{/each}
	</Body>
</DataTable>
