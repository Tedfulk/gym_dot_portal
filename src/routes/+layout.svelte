<script lang="ts">
	import { mdiWeatherNight, mdiWeatherSunny, mdiAccountCircle } from '@mdi/js';
	import TopAppBar, { Row, Section, Title, AutoAdjust } from '@smui/top-app-bar';
	import IconButton, { Icon } from '@smui/icon-button';
	import { Svg } from '@smui/common';
	import { onMount } from 'svelte';
	import Drawer, { AppContent, Content, Header, Subtitle } from '@smui/drawer';
	import Button, { Label } from '@smui/button';
	import List, { Item, Text } from '@smui/list';

	let open: boolean = false;
	let active: string;
	let username: string = 'Ted Fulk';
	let motto: string = 'Be Penomenal or Be Forgotten';

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
				<Icon component={Svg} viewBox="0 0 22 22">
					<path fill="currentColor" d={mdiAccountCircle} />
				</Icon>
			</IconButton>
			<Button href="/">
				<Title>Gym Dot</Title>
			</Button>
		</Section>
		<Section align="end" toolbar>
			<Button href="myschedule"><Label>My Schedule</Label></Button>
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
	<div class="drawer-container" style="margin-left: 1rem;">
		<Drawer variant="dismissible" bind:open>
			<Header>
				<Title>{username}</Title>
				<Subtitle>{motto}</Subtitle>
			</Header>
			<Content>
				<List>
					<Item
						href="profile"
						on:click={() => setActive('profile')}
						activated={active === 'profile'}
					>
						<Text>Profile</Text>
					</Item>
					<Item
						href="accountinfo"
						on:click={() => setActive('account info')}
						activated={active === 'account info'}
					>
						<Text>Account Info</Text>
					</Item>
					<Item
						href="prboard"
						on:click={() => setActive('pr board')}
						activated={active === 'pr board'}
					>
						<Text>PR Board</Text>
					</Item>
					<Item
						href="attendance"
						on:click={() => setActive('attendance')}
						activated={active === 'attendance'}
					>
						<Text>Attendance</Text>
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
				<slot />
			</main>
		</AppContent>
	</div>
</AutoAdjust>

<style>
</style>
