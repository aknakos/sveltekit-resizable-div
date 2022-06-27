<script>
	import ResizableDiv from '$lib/ResizableDiv.svelte';

	function click() {} //called when resize is clicked
	function release() {} //called when resize is released
	function move() {} //called when size changes during resizing
</script>

<h1>Welcome to SvelteKit</h1>
<ResizableDiv
	on:click={click}
	on:release={release}
	on:move={move}
	class="inside-resizable"
	position="se"
>
	<div>
		Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation
	</div>
</ResizableDiv>

<style>
	:global.inside-resizable {
		width: 10rem; /* default width*/
		height: 10rem; /* default height */

		/* positioning */
		position: relative;
		top: 0;
		left: 0;

		/* extra stuff */
		background-color: aquamarine;
	}
</style>
