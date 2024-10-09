<script lang="ts">
	import { onMount } from 'svelte';

	const hours = Array.from({ length: 24 }, (value, index) => index);
	const minutes = Array.from({ length: 60 }, (value, index) => index);

	let wheelContainer: HTMLDivElement;
	let item: HTMLElement;
	let selectedHour = 0;

	function isElementInViewport(el: HTMLElement, container: HTMLDivElement): Boolean {
		var rect = el.getBoundingClientRect();
		return (
			rect.top >= 0 &&
			rect.left >= 0 &&
			rect.bottom <= (container.offsetHeight || document.documentElement.clientHeight) &&
			rect.right <= (container.offsetWidth || document.documentElement.clientWidth)
		);
	}

	onMount(() => {
		// const item = document.getElementById('3');
		console.log(isElementInViewport(item, wheelContainer));
	});
</script>

<div class="flex items-center justify-center">
	<div
		bind:this={wheelContainer}
		<!--
		on:scroll={console.log(isElementInViewport(item, wheelContainer))}
		--
	>
		id="wheelContainer" class="wheel-container h-[300px] w-[350px] snap-y space-y-8
		overflow-y-scroll py-[300px] text-5xl font-bold text-black" >
		{#each hours as hour}
			{#if hour === 3}
				<div bind:this={item} id={hour} class="wheel-item snap-center text-center">{hour}</div>
			{:else}
				<div id={hour} class="wheel-item snap-center text-center">{hour}</div>
			{/if}
		{/each}
	</div>
</div>

<p>Selected hour: {selectedHour}</p>

<style>
	.wheel-item.centered {
		color: red;
		transform: scale(1.2);
		transition: all 0.3s ease;
	}
</style>
