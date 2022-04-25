<script context="module">
	export async function load({ fetch, params }) {
		const baseUrl = `https://henryyyyyyyyyy.herokuapp.com/`;
		const stock = 'mrna';
		const response = await fetch(baseUrl + stock + '?key=jamesiscool');
		const data = await response.json();

		if (data) {
			return {
				props: {
					stocks: data
				}
			};
		}
	}
</script>

<script>
	import StockList from '../components/StockList.svelte';

	export let stocks;
	console.log(stocks);
</script>

<section class="flex flex-column relative">
	{#if stocks.length > 0}
		<StockList {stocks} />
	{/if}
</section>
