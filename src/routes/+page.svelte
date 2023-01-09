<script lang="ts">
	import Rounds from './../lib/rounds.svelte';
	import { Styles } from 'sveltestrap';
	import { Row, Col, Container, Icon } from 'sveltestrap/src';
	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import AudioPlayer from '$lib/AudioPlayer.svelte';

	let counter = 180;

	let countdown = 180;
	let countdownInterval: string | number | NodeJS.Timeout | undefined;

	let activeRound = 0;
	let rounds = 5;

	let tracks = ['countdown.mp3', 'background_music.mp3', 'kazoo.mp3'];

	let audioPlayerBeep: AudioPlayer;
	let audioPlayerBackground: AudioPlayer;
	let audioPlayerFinish: AudioPlayer;

	function stopCountdown() {
		clearInterval(countdownInterval);

		audioPlayerBackground.stop();
	}

	function startCountdown() {
		counter = countdown;

		audioPlayerBackground.play();
		audioPlayerBackground.setVolume(0.2);
		countdownInterval = setInterval(decrement, 1000);
	}

	function backwards() {
		if (activeRound > 0) {
			activeRound--;
		}
	}

	function forwards() {
		if (activeRound < rounds - 1) {
			activeRound++;
		}
	}

	onMount(async () => {
		counter = countdown;
	});
	function decrement() {
		if (counter <= 15) {
			toColor.set([215 + counter * 2, 50, 34]);
		} else {
			toColor.set([215, 50, 34]);
		}
		if (counter == 11) {
			audioPlayerBeep.play();
		}
		if (counter <= 25 && counter > 5) {
			audioPlayerBackground.decVolume(0.005);
		}

		if (counter == 5) {
			audioPlayerBackground.stop();
		}
		if (counter == 1) {
			audioPlayerFinish.play();
		}
		if (counter <= 1) {
			clearInterval(countdownInterval);
		}
		counter--;
	}
	const toColor = tweened([255, 0, 0], {
		duration: 1000,
		easing: cubicOut
	});
</script>

<Styles />

<AudioPlayer src={tracks[0]} bind:this={audioPlayerBeep} />
<AudioPlayer src={tracks[1]} bind:this={audioPlayerBackground} />
<AudioPlayer src={tracks[2]} bind:this={audioPlayerFinish} />

<div class="gradient">
	<Container>
		<Row style="padding-top: 30px">
			<Col sm="4"><Rounds {rounds} {activeRound} /></Col>
			<Col sm="8" class="">
				<Row class="justify-content-center">
					<Col sm="auto">
						<button on:click={backwards}>
							<Icon name="arrow-left-circle" />
						</button>
					</Col>

					<Col sm="auto"><button on:click={startCountdown}>Start</button></Col>
					<Col sm="auto"><button on:click={stopCountdown}>Stop</button></Col>
					<Col sm="auto">
						<button on:click={forwards}>
							<Icon name="arrow-right-circle" />
						</button>
					</Col>
				</Row>
				<Row>
					{#if counter == 0}
						<div class=" counter d-flex justify-content-center end">ENDE</div>
					{:else}
						<div
							class={(counter < 10 ? 'fast' : 'slow') + ' counter d-flex justify-content-center'}
						>
							{counter}s
						</div>
					{/if}
				</Row>
			</Col>
		</Row>
	</Container>
</div>

<style>
	.fast {
		-webkit-animation: glow 0.1s ease-in-out infinite alternate;
		-moz-animation: glow 1s ease-in-out infinite alternate;
		animation: glow 0.3s ease-in-out infinite alternate;
	}
	.slow {
		-webkit-animation: glow 1s ease-in-out infinite alternate;
		-moz-animation: glow 1s ease-in-out infinite alternate;
		animation: glow 2s ease-in-out infinite alternate;
	}

	.end {
		text-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #ff6026, 0 0 40px #ff6026, 0 0 50px #ff6026,
			0 0 60px #ff6026, 0 0 70px #ff6026;

		-webkit-animation: blinks 1s ease-in-out infinite alternate;
		-moz-animation: blinks 0.1s ease-in-out infinite alternate;
		animation: blinks 0.7s ease-in-out infinite alternate;
	}
	.counter {
		margin-top: 50px;
		font-size: 1000%;
		color: #ffc75f;
		/*border: solid 2px #ff6026;*/
		/* -webkit-text-stroke: 3px #ff6026; width and color */

		font-family: 'Helvetica Neue', sans-serif;
		font-size: 275px;
		font-weight: bold;
		letter-spacing: -1px;
		line-height: 1;
		text-align: center;
	}
	@keyframes blinks {
		from {
			font-size: 1000%;
			color: #ff6026; /*width and color */
			opacity: 0.5;
		}
		to {
			font-size: 1300%;
		}
	}

	@keyframes glow {
		from {
			text-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #ff6026, 0 0 40px #ff6026,
				0 0 50px #ff6026, 0 0 60px #ff6026, 0 0 70px #ff6026;
		}
		to {
			text-shadow: 0 0 15px #fff, 0 0 25px #ffc75f, 0 0 35px #ffc75f, 0 0 45px #ffc75f,
				0 0 55px #ffc75f, 0 0 65px #ffc75f, 0 0 75px #ffc75f;
		}
	}
	button {
		background-color: #ffc75f;
		/*border: solid 2px #ff6026;*/
		border: none;
		color: black;
		padding: 5px;
		border-radius: 5px;
		font-size: 200%;
	}

	.gradient {
		height: 100vh;
		background: rgba(52, 23, 95, 1);
		background: -webkit-linear-gradient(top left, rgba(52, 23, 95, 1), rgba(218, 65, 93, 1));
		background: -moz-linear-gradient(top left, rgba(52, 23, 95, 1), rgba(218, 65, 93, 1));
		background: linear-gradient(to bottom right, rgba(52, 23, 95, 1), rgba(218, 65, 93, 1));
	}
</style>
