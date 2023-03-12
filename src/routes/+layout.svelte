<script lang="ts">
	import { mdiWeatherNight, mdiWeatherSunny, mdiMenu } from '@mdi/js';
	import TopAppBar, { Row, Section, Title, AutoAdjust } from '@smui/top-app-bar';
	import IconButton, { Icon } from '@smui/icon-button';
	import { Svg } from '@smui/common';
	import { onMount } from 'svelte';
	import Drawer, { AppContent, Content, Header, Subtitle } from '@smui/drawer';
	import Button, { Label } from '@smui/button';
	import List, { Item, Text } from '@smui/list';

	let open: boolean = false;
	let active = 'Gray Kittens';

	let topAppBar: TopAppBar;
	let darkTheme: boolean | undefined = undefined;

	function setActive(value: string) {
		active = value;
	}

	onMount(() => {
		darkTheme = window.matchMedia('prefers-color-scheme: dark').matches;
	});
</script>

<svelte:head>
	<!-- SMUI Styles -->
	{#if $darkTheme === 'undefined'}
		<link rel="stylesheet" href="/smui.css" media="(prefers-color-scheme: light)" />
		<link rel="stylesheet" href="/smui-dark.css" media="screen and (prefers-color-scheme: dark)" />
	{:else if darkTheme}
		<link rel="stylesheet" href="/smui.css" />
		<link rel="stylesheet" href="/smui-dark.css" media="screen" />
	{:else}
		<link rel="stylesheet" href="/smui.css" />
	{/if}
</svelte:head>

<TopAppBar bind:this={topAppBar} variant="standard" dense>
	<Row>
		<Section>
			<IconButton on:click={() => (open = !open)}>
				<Icon component={Svg} viewBox="0 0 24 24">
					<path fill="currentColor" d={mdiMenu} />
				</Icon>
			</IconButton>
			<Title>Gym Dot</Title>
		</Section>
		<Section align="end" toolbar>
			<IconButton
				on:click={() => {
					darkTheme = !darkTheme;
				}}
				title={darkTheme ? 'Lights on' : 'Lights off'}
			>
				<Icon component={Svg} viewBox="0 0 24 24">
					<path fill="currentColor" d={darkTheme ? mdiWeatherSunny : mdiWeatherNight} />
				</Icon>
			</IconButton>
		</Section>
	</Row>
</TopAppBar>
<AutoAdjust {topAppBar}>
	<div class="drawer-container">
		<Drawer variant="dismissible" bind:open>
			<Header>
				<Title>Super Drawer</Title>
				<Subtitle>It's the best drawer.</Subtitle>
			</Header>
			<Content>
				<List>
					<Item
						href="javascript:void(0)"
						on:click={() => setActive('Gray Kittens')}
						activated={active === 'Gray Kittens'}
					>
						<Text>Gray Kittens</Text>
					</Item>
					<Item
						href="javascript:void(0)"
						on:click={() => setActive('A Space Rocket')}
						activated={active === 'A Space Rocket'}
					>
						<Text>A Space Rocket</Text>
					</Item>
					<Item
						href="javascript:void(0)"
						on:click={() => setActive('100 Pounds of Gravel')}
						activated={active === '100 Pounds of Gravel'}
					>
						<Text>100 Pounds of Gravel</Text>
					</Item>
					<Item
						href="javascript:void(0)"
						on:click={() => setActive('All of the Shrimp')}
						activated={active === 'All of the Shrimp'}
					>
						<Text>All of the Shrimp</Text>
					</Item>
					<Item
						href="javascript:void(0)"
						on:click={() => setActive('A Planet with a Mall')}
						activated={active === 'A Planet with a Mall'}
					>
						<Text>A Planet with a Mall</Text>
					</Item>
				</List>
			</Content>
		</Drawer>

		<AppContent class="app-content">
			<main class="main-content">
				<slot class="status">Active: {active}</slot>
			</main>
		</AppContent>
	</div>
</AutoAdjust>
