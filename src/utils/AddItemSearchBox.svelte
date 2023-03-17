<script lang="ts">
	import Autocomplete from '@smui-extra/autocomplete';
	import { Text } from '@smui/list';
	import Button, { Label } from '@smui/button';
	import Dialog, { Title, Content, Actions } from '@smui/dialog';
	import Textfield from '@smui/textfield';
	import { options } from '../routes/prboard/DataTable.svelte';
	import type { PRExercise } from '../routes/prboard/DataTable.svelte';

	let dialogOpen: boolean = false;
	let newName: string = '';
	let newRepScheme: number;

	let value: PRExercise[];
	let listOfExercises: PRExercise[];
	let text: string = '';

	function addObject() {
		const newObject = {
			id: options[options.length].id + 1,
			name: newName,
			repScheme: newRepScheme
		};
		listOfExercises = [...options, ...newObject];
		value = newObject;
		dialogOpen = false;
	}
</script>

<div>
	<Autocomplete
		{options}
		getOptionLabel={(option) => (option ? `${option.name}` : '')}
		bind:value
		bind:text
		noMatchesActionDisabled={false}
		on:SMUIAutocomplete:noMatchesAction={() => {
			newName = text;
			dialogOpen = true;
		}}
		label="Record"
	>
		<div slot="no-matches">
			<Text>Add item</Text>
		</div>
	</Autocomplete>

	<pre class="status">Selected: {value ? JSON.stringify(value) : ''}</pre>

	<Dialog
		bind:open={dialogOpen}
		aria-labelledby="autocomplete-dialog-title"
		aria-describedby="autocomplete-dialog-content"
	>
		<Title id="autocomplete-dialog-title">New Item</Title>
		<Content id="autocomplete-dialog-content">
			<Textfield bind:value={newName} label="Label" />
		</Content>
		<Actions>
			<Button>
				<Label>Cancel</Label>
			</Button>
			<Button on:click={addObject}>
				<Label>Add</Label>
			</Button>
		</Actions>
	</Dialog>
</div>
