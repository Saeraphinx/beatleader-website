<script>
	import {configStore} from '../../stores/config';
	import {fly, fade} from 'svelte/transition';
	import RankingTable from '../Ranking/RankingTable.svelte';
	import Switch from '../Common/Switch.svelte';

	export let animationSign = 1;
	export let visible = false;

	let currentShowClans = true;
	let currentShowDifference = true;
	let currentShowFriendsButton = true;
	let currentShowCountryRank = true;
	let currentShowColorsForCountryRank = true;
	let currentPPToTheLeft = false;

	function onConfigUpdated(config) {
		if (config?.rankingList?.showClans != currentShowClans) currentShowClans = config?.rankingList?.showClans ?? true;
		if (config?.rankingList?.showFriendsButton != currentShowFriendsButton)
			currentShowFriendsButton = config?.rankingList?.showFriendsButton ?? true;
		if (config?.rankingList?.showDifference != currentShowDifference) currentShowDifference = config?.rankingList?.showDifference ?? true;
		if (config?.rankingList?.showCountryRank != currentShowCountryRank)
			currentShowCountryRank = config?.rankingList?.showCountryRank ?? true;
		if (config?.rankingList?.ppToTheLeft != currentPPToTheLeft) currentPPToTheLeft = config?.rankingList?.ppToTheLeft ?? true;
	}

	async function settempsetting(key, value) {
		var rankingList = configStore.get('rankingList');
		rankingList[key] = value;
		await configStore.setForKey('rankingList', rankingList, false);
	}

	$: onConfigUpdated(configStore && $configStore ? $configStore : null);

	$: settempsetting('showFriendsButton', currentShowFriendsButton);
	$: settempsetting('showClans', currentShowClans);
	$: settempsetting('showDifference', currentShowDifference);
	$: settempsetting('showCountryRank', currentShowCountryRank);
	$: settempsetting('showColorsForCountryRank', currentShowColorsForCountryRank);
	$: settempsetting('ppToTheLeft', currentPPToTheLeft);
</script>

<div class="main-container" class:visible in:fly|global={{y: animationSign * 200, duration: 400}} out:fade|global={{duration: 100}}>
	<div class="profile">
		<RankingTable page={1} meta={false} editing={true} />
	</div>

	<section class="option">
		<label title="Determines when to show the buttons">Options</label>
		<div class="switches">
			<Switch
				value={currentShowFriendsButton}
				label="Add to friends button"
				fontSize={12}
				design="slider"
				on:click={() => (currentShowFriendsButton = !currentShowFriendsButton)} />
			<Switch
				value={currentShowClans}
				label="Clans"
				fontSize={12}
				design="slider"
				on:click={() => (currentShowClans = !currentShowClans)} />
			<Switch
				value={currentShowDifference}
				label="Rank Difference"
				fontSize={12}
				design="slider"
				on:click={() => (currentShowDifference = !currentShowDifference)} />
			<Switch
				value={currentShowCountryRank}
				label="Show Country"
				fontSize={12}
				design="slider"
				on:click={() => (currentShowCountryRank = !currentShowCountryRank)} />
			<Switch
				value={currentShowColorsForCountryRank}
				label="Podium rank colors"
				fontSize={12}
				design="slider"
				on:click={() => (currentShowColorsForCountryRank = !currentShowColorsForCountryRank)} />
			<!-- <Switch
					value={currentPPToTheLeft}
					label="PP on the left"
					fontSize={12}
					design="slider"
					on:click={() => (currentPPToTheLeft = !currentPPToTheLeft)} /> -->
		</div>
	</section>
</div>

<style>
	.main-container {
		display: none;
		flex-direction: column;
	}

	.main-container.visible {
		display: flex;
	}

	.profile {
		max-width: 67em;
		overflow: auto;
		max-height: 22.3em;
		border: 3px dashed var(--textColor);
		padding-top: 0.3em;
		scrollbar-width: none;
	}

	.profile::-webkit-scrollbar {
		display: none;
	}

	.options {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		grid-gap: 1em;
		align-items: start;
		justify-items: start;
		margin-top: 1rem;
	}

	.option {
		display: flex;
		flex-direction: column;
		width: 100%;
	}

	label {
		display: block;
		font-size: 0.75em;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		color: #afafaf !important;
		margin-bottom: 0.25em;
	}
	.switches {
		display: flex;
		grid-gap: 1em;
		flex-wrap: wrap;
		justify-content: space-evenly;
		padding: 0.5em;
	}

	@media screen and (max-width: 600px) {
		.options {
			grid-template-columns: 1fr;
		}
		.switches {
			display: grid;
		}
	}
</style>
