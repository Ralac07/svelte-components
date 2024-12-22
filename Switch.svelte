<script lang="ts">
	import tinycolor from 'tinycolor2'; // npm install tinycolor2; npm install --save @types/tinycolor2
	import cssVars from 'svelte-css-vars'; // npm i svelte-css-vars
	export let id: string | undefined = undefined;
	export let name: string | undefined = undefined;
	export let scale: number = 1;
	export let state: boolean = false;
	export let aspect_ratio = 2;
	export let offColor = 'silver';
	export let onColor = 'dodgerblue';
	export let useShadows = true;
	const h = `calc(1em * ${scale ?? 1})`;
	$: styleVars = {
		offColor,
		onColor,
		aspect_ratio,
		h,
		state: state ? 1 : 0,
		offColorShadow: useShadows ? tinycolor(offColor).darken(7) : offColor,
		onColorShadow: useShadows ? tinycolor(onColor).darken(7) : onColor
	};
</script>

<main use:cssVars={styleVars}>
	<input
		type="checkbox"
		style="display: none;"
		bind:checked={state}
		{...id !== undefined ? { id } : {}}
		{...name !== undefined ? { name } : {}}
	/>
	<div
		on:click={(event) => {
			state = !state;
		}}
		id="switch"
	>
		<div id="slider">
			<div id="slideHandle"></div>
		</div>
	</div>
</main>

<style lang="scss">
	#switch {
		height: var(--h);
		aspect-ratio: var(--aspect_ratio);
		background-color: var(--offColor);
		position: relative;
		overflow: hidden;
		border-radius: var(--h);
	}
	#slider {
		height: var(--h);
		border-radius: 0 var(--h) var(--h) 0;
		aspect-ratio: var(--aspect_ratio);
		background-color: var(--onColor);
		position: absolute;
		left: calc(var(--h) * (1 - var(--state)) * -1);
		transition: left 300ms;
		box-shadow: 10px 10px 10px 7px var(--offColorShadow);
		overflow: hidden;
	}
	#slideHandle {
		box-shadow: 5px 2px 10px 10px var(--onColorShadow);
		border-radius: 50%;
		background-color: white;
		aspect-ratio: 1;
		position: absolute;
		right: 0;
		transform: scale(0.9);
		height: var(--h);
		overflow: hidden;
	}
</style>
