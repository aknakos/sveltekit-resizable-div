# Simple resizable div for svelte

Simple implementation of a resizable div for svelte. Only supports NE resizing for now.
I made this to easily reuse the logic, but feel free to copy the code in the .svelte file to customize however you want and/or if you don't want to install a dependency only for this.

This is a work in progress. The position property can be one of 4 cardinal directions (se, ne, sw, nw) and you need to setup the css positioning properly as exemplified below.

Usage:

```javascript
<script>
	import ResizableDiv from '@aknakos/sveltekit-resizable-div';

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
```
