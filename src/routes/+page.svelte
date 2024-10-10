<script lang="ts">
	import { onMount } from 'svelte';

	const hours = Array.from({ length: 24 }, (value, index) => index);
	const minutes = Array.from({ length: 60 }, (value, index) => index);

	let wheelContainer: HTMLDivElement;
	let item_3: HTMLElement;
	// let item: HTMLElement;
	let selectedHour = 0;

	function isElementInViewport(el: HTMLElement, container: HTMLDivElement): Boolean {
		var elRect = el.getBoundingClientRect();
		var containerRect = container.getBoundingClientRect();
		return (
			elRect.top + elRect.height - containerRect.top > 0 &&
			elRect.top <= containerRect.top + containerRect.height
		);
	}

	function offsetToTop(el: HTMLElement, container: HTMLDivElement) {
		var elRect = el.getBoundingClientRect();
		var containerRect = container.getBoundingClientRect();
		return elRect.top - containerRect.top;
	}

	function offsetToCenter(el: HTMLElement, container: HTMLDivElement): number {
		var elRect = el.getBoundingClientRect();
		var containerRect = container.getBoundingClientRect();
		return elRect.top + elRect.height / 2 - containerRect.top - containerRect.height / 2;
	}

	function getX(y: number): number {
		return Math.sin(Math.acos(Math.abs(y) / 300)) * 300;
	}

	function getTh(y: number): number {
		return (Math.asin(Math.abs(y) / 300) * 180) / Math.PI;
	}

	onMount(() => {
		const item: HTMLElement = document.getElementById('5');
		function applyTranform() {
			const children = wheelContainer.children;
			for (let i = 0; i < children.length; i++) {
				const child = children[i] as HTMLElement;
				child.style.transform = `scale(${getX(offsetToCenter(child, wheelContainer)) / 300})`;
				child.style.rotate = `x ${getTh(offsetToCenter(child, wheelContainer))}deg`;
			}
		}
		// event type, listener, options
		wheelContainer.addEventListener('scroll', applyTranform);
	});
</script>

<div class="flex items-center justify-center p-32">
	<div
		bind:this={wheelContainer}
		class="wheel-container h-[300px] w-[350px] snap-y space-y-8 overflow-y-scroll py-[300px] text-5xl font-bold text-black"
	>
		{#each hours as hour}
			{#if hour === 5}
				<div id={hour} class="wheel-item snap-center text-center">
					{hour}
				</div>
			{:else}
				<div id={hour} class="wheel-item snap-center text-center">{hour}</div>
			{/if}
		{/each}
	</div>
</div>

<p>Selected hour: {selectedHour}</p>

<style>
	.wheel-container {
		transform-style: preserve-3d;
		transform: perspective(300px);
	}
</style>
