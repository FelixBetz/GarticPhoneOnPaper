<script lang="ts">
	import { Icon, Col } from 'sveltestrap';
	export let num = 0;
	export let isActive = false;
</script>

<div class={(isActive ? 'active' : 'inactive') + ' item' + (num % 2 == 0 ? ' even' : ' odd')}>
	<div class="row justify-content-md-center">
		<Col class="d-flex justify-content-center" sm="3">
			{num} .
		</Col>
		<Col class="d-flex justify-content-center" sm="6">
			{num % 2 == 0 ? 'Zeichnen' : 'Schreiben'}
		</Col>
		<Col class="d-flex justify-content-center" sm="3">
			<Icon name={num % 2 == 0 ? 'card-image' : 'pencil'} />
		</Col>
	</div>
</div>

<style>
	.active {
		border: solid 8px #ff6026;
		opacity: 100%;
		font-size: 250%;
		font-weight: 600;
	}
	.inactive {
		border: solid 2px #ffc75f;
		opacity: 80%;
		font-size: 200%;
	}
	.odd {
		background-color: #ffc75f;
	}
	.even {
		background-color: #f9f871;
	}
	.item {
		padding: 5px;
		border-radius: 15px;

		color: black;
	}
</style>
