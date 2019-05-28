<script>
	import * as GL from '@sveltejs/gl';
	import { onMount } from 'svelte';

	let num = 200;

	$: boxes = new Array(num)
		.fill()
		.map(() => [
			Math.random() * 360,
			Math.random() * 360,
			Math.random() * 360
		]);

	onMount(() => {
		let running = true;

		const loop = () => {
			if (!running) return;
			requestAnimationFrame(loop);

			for (let i = 0; i < boxes.length; i += 1) {
				boxes[i][0] += 0.3;
				boxes[i][1] += 0.3;
				boxes[i][2] += 0.3;
			}
		};

		loop();

		return () => running = false;
	});
</script>

<GL.Scene>
	<GL.Target id="origin" location={[0,0,0]}/>
	<GL.PerspectiveCamera location={[0,0,5]} target="origin" fov={30}/>

	<GL.DirectionalLight
		direction={[0.2,-0.2,-1]}
	/>

	{#each boxes as box}
		<GL.Box color={0xff3e00} rotation={box}/>
	{/each}
</GL.Scene>

<div class="controls">
	<label>
		<input type="number" bind:value={num} min={1} max={2500}>
		<input type="range" bind:value={num} min={1} max={2500}>
		number of boxes
	</label>
</div>

<style>
	.controls {
		position: absolute;
		top: 1em;
		left: 1em;
	}
</style>