<script lang="ts">
	import { onMount } from 'svelte';

	const hours = Array.from({ length: 24 }, (value, index) => index);
	const minutes = Array.from({ length: 60 }, (value, index) => index);

	let wheelContainer: HTMLDivElement;

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

	function getX(y: number, r: number): number {
		r *= 1.25;
		return Math.sin(Math.acos(Math.abs(y) / r)) * r;
	}

	function getTh(y: number, r: number): number {
		r *= 1.25;
		return (Math.asin(Math.abs(y) / r) * 180) / Math.PI;
	}

	onMount(() => {
		const diam = wheelContainer.getBoundingClientRect().height;
		function logger() {
			const item = document.getElementById('9');
			if (item) {
				console.log('scale:', getX(offsetToCenter(item, wheelContainer), diam / 2) / (diam / 2));
				console.log('angle:', getTh(offsetToCenter(item, wheelContainer), diam / 2));
			}
		}
		const children = wheelContainer.children;
		for (let i = 0; i < 24; i++) {
			const child = children[i] as HTMLElement;
			child.style.transform = `scale(${getX(offsetToCenter(child, wheelContainer), diam / 2) / (diam / 2)})`;
			child.style.rotate = `x ${getTh(offsetToCenter(child, wheelContainer), diam / 2)}deg`;
		}
		function applyTranform() {
			const children = wheelContainer.children;
			for (let i = 0; i < 24; i++) {
				const child = children[i] as HTMLElement;
				child.style.transform = `scale(${getX(offsetToCenter(child, wheelContainer), diam / 2) / (diam / 2)})`;
				child.style.rotate = `x ${getTh(offsetToCenter(child, wheelContainer), diam / 2)}deg`;
			}
		}
		wheelContainer.addEventListener('scroll', applyTranform);
		wheelContainer.addEventListener('scroll', logger);
	});
</script>

<div class="flex items-center justify-center p-32">
	<div
		bind:this={wheelContainer}
		class="wheel-container h-[100px] w-[55px] space-y-3 overflow-y-scroll py-[100px] text-3xl font-bold text-black"
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

<input type="time" />
<p>Selected hour: {selectedHour}</p>

<style>
	.wheel-container {
		scroll-snap-type: y proximity;
		transform-style: preserve-3d;
		transform: perspective(300px);
		-ms-overflow-style: none; /* Internet Explorer 10+ */
		scrollbar-width: none; /* Firefox */
	}

	.wheel-container::-webkit-scrollbar {
		display: none; /* Safari and Chrome */
	}

	.wheel-item {
		transform: scale(0);
		rotate: x 0deg;
	}
</style>
