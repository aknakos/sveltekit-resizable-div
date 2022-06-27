<script lang="ts">
	import { createEventDispatcher, onMount } from 'svelte';
	const dispatch = createEventDispatcher();

	interface MousePosI {
		x: number;
		y: number;
	}
	interface SizeI {
		width: number;
		height: number;
	}
	enum Direction {
		NE = 'ne',
		NW = 'nw',
		SE = 'se',
		SW = 'sw'
	}

	export let position: Direction = Direction.NE;

	let width: number;
	let height: number;
	let ref: Element;
	let dragging = false;
	let mousepos: MousePosI | null = null;
	let originalSize: SizeI | null = null;
	let extrawidth = 0;
	let extraheight = 0;

	let padding = 10;

	$: height = originalSize ? originalSize.height + extraheight : 0;
	$: width = originalSize ? originalSize.width + extrawidth : 0;

	onMount(() => {
		originalSize = ref.getBoundingClientRect();
	});

	function onClick(e: MouseEvent) {
		dragging = true;
		dispatch('click');
		originalSize = ref.getBoundingClientRect();
		mousepos = null;
		extrawidth = 0;
		extraheight = 0;
	}

	function onRelease(e: MouseEvent) {
		if (dragging) {
			dispatch('release');
			mousepos = null;
			dragging = false;
		}
	}

	function handleMousemove(e: MouseEvent) {
		if (dragging) {
			if (mousepos === null) {
				mousepos = { x: e.x, y: e.y };
			} else {
				switch (position) {
					case Direction.NE:
						extraheight += -e.y + mousepos.y;
						extrawidth += e.x - mousepos.x;
						break;
					case Direction.NW:
						extraheight += -e.y + mousepos.y;
						extrawidth -= e.x - mousepos.x;
						break;
					case Direction.SE:
						extraheight -= -e.y + mousepos.y;
						extrawidth += e.x - mousepos.x;
						break;
					case Direction.SW:
						extraheight -= -e.y + mousepos.y;
						extrawidth -= e.x - mousepos.x;
						break;
				}
				mousepos = { x: e.x, y: e.y };
				dispatch('move');
			}
		}
	}
</script>

{#if dragging}
	<div
		class="resizable-overlay"
		on:mousemove|preventDefault|stopPropagation={handleMousemove}
		on:mouseup={onRelease}
	/>
{/if}

<div
	{...$$restProps}
	class={'resizable-div ' + $$props.class}
	bind:this={ref}
	style={originalSize ? `height:${height - padding * 2}px;width:${width - padding * 2}px` : ''}
>
	<slot />
	<span
		class={`resizable-handle resizable-handle-` + position}
		on:mousedown|preventDefault|stopPropagation={onClick}
	/>
</div>

<style>
	.resizable-handle-ne {
		top: 0;
		right: 0;
		cursor: ne-resize;
		transform: rotate(270deg);
	}
	.resizable-handle-se {
		bottom: 0;
		right: 0;
		cursor: se-resize;
		transform: rotate(0deg);
	}
	.resizable-handle-nw {
		top: 0;
		left: 0;
		cursor: nw-resize;
		transform: rotate(180deg);
	}
	.resizable-handle-sw {
		bottom: 0;
		left: 0;
		cursor: sw-resize;
		transform: rotate(90deg);
	}

	.resizable-handle {
		position: absolute;
		width: 20px;
		height: 20px;
		background-repeat: no-repeat;
		background-origin: content-box;
		box-sizing: border-box;
		background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHN0eWxlPSJiYWNrZ3JvdW5kLWNvbG9yOiNmZmZmZmYwMCIgd2lkdGg9IjYiIGhlaWdodD0iNiI+PHBhdGggZD0iTTYgNkgwVjQuMmg0LjJWMEg2djZ6IiBvcGFjaXR5PSIuMzAyIi8+PC9zdmc+);
		background-position: 100% 100%;
		padding: 0 3px 3px 0;
	}

	.resizable-div {
		padding: 10px;
	}

	.resizable-overlay {
		width: 100vw;
		height: 100vh;
		top: 0px;
		right: 0px;
		bottom: 0px;
		left: 0px;
		/* background-color: rgba(0, 0, 0, 0.5); */
		position: fixed;
		z-index: 999999999;
	}
</style>
