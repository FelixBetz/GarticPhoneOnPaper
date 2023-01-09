<script lang="ts">
	import { onMount } from 'svelte';

	export let src: string = '';

	let player: HTMLAudioElement;

	onMount(() => {});

	export function play() {
		player.currentTime = 0;
		player.play();
	}
	export function stop() {
		player.pause();
	}
	export function setVolume(volume: number) {
		player.volume = volume;
	}
	export function decVolume(volume: number) {
		player.volume -= volume;
	}
</script>

<div>
	<audio bind:this={player} {src} hidden />
</div>
