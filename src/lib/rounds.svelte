<script lang="ts">
	import { Row } from 'sveltestrap';
	import Round from './round.svelte';
	export let rounds = 3;
	export let activeRound = 1;
</script>

{#each Array(rounds) as _, i}
	<div style="margin-bottom: 30px">
		<Round num={i + 1} isActive={activeRound == i} />
	</div>
{/each}

<Row />
