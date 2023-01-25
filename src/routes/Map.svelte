<script>
	import { onMount, onDestroy, setContext } from 'svelte';
	import { Map, NavigationControl, ScaleControl } from 'maplibre-gl';
	import { browser } from '$app/environment';
	import 'maplibre-gl/dist/maplibre-gl.css';

	let mapContainer;
	let map;
	let loaded = false;

	let setCamera = !window.location.hash;

	setContext('map', { getMap: () => map, setCamera: setCamera });

	onMount(() => {
		map = new Map({
			container: mapContainer,
			style: 'https://demotiles.maplibre.org/style.json',
			center: [-74.5, 40],
			zoom: 9
		});

		map.addControl(new ScaleControl());
		map.addControl(new NavigationControl(), 'bottom-left');

		map.on('load', () => {
			loaded = true;
		});
	});
	onDestroy(() => console.log('Wrap up'));
</script>

<div class="map" bind:this={mapContainer}>
	{#if loaded}
		<slot />
	{/if}
</div>

<style>
	.map {
		position: relative;
		flex-grow: 1;
		min-height: 100vh;
	}
</style>
