<script context="module">
	import StockStore from '../stocks';
	let stocks = [];
	const unsub = StockStore.subscribe((val) => {
		stocks = val;
	});
	unsub();
	export async function load({ fetch, params }) {
		const baseUrl = `https://henryyyyyyyyyy.herokuapp.com/`;
		const data = await Promise.all(
			stocks.map(async (stock) => {
				const res = await fetch(baseUrl + stock + '?key=jamesiscool');
				return await res.json();
			})
		);

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

	import { onMount } from 'svelte';
	onMount(() => {
		// console.log($StockStore);
	});

	$: getData($StockStore);
	async function getData(newTickers) {
		const oldTickers = stocks.map((val) => Object.keys(val)[0]);
		if (newTickers.length < oldTickers.length) {
			return (stocks = stocks.filter((val) => newTickers.includes(Object.keys(val)[0])));
		}
		let difference = newTickers.filter((x) => !oldTickers.includes(x));
		const baseUrl = `https://henryyyyyyyyyy.herokuapp.com/`;
		const data = await Promise.all(
			difference.map(async (stock) => {
				const res = await fetch(baseUrl + stock + '?key=jamesiscool');
				return await res.json();
			})
		);
		stocks = [...data, ...stocks].filter((stock) => {
			const ticker = Object.keys(stock)[0];
			return stock[ticker]?.prices?.length > 50;
		});
	}
</script>

<section class="flex flex-column relative">
	{#if stocks.length > 0}
		<StockList {stocks} />
	{/if}
</section>
