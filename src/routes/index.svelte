<!-- https://www.youtube.com/watch?v=uGhiM19d950 -->
<script>
import { onMount } from "svelte";


	let coins = [];
	let filteredCoins = [];
	let titles = [
		'#',
		'Coin',
		'Price',
		'Price Change',
		'24h Volume'
	];
	let ref = null;

	const loadCoins = async () => {
	const resp = await fetch(
		"https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
	);
	const data = await resp.json();
	// console.log(data);
	coins = data;
	filteredCoins = data;
	};

	loadCoins();

	const searchCoin = (value) => {
		filteredCoins = coins.filter(coin => 
			coin.name.toLowerCase().includes(value.toLowerCase()) || 
			coin.symbol.toLowerCase().includes(value.toLowerCase())
			)
	}

	onMount(() => {
		ref.focus();
	})
</script>

<div class="container">
  <div class="row">
    <h1>CoinMarket</h1>
	<input 
		type="text" 
		class="form-control bg-dark text-white rounded-0 border-0 my-4" 
		placeholder="Search your Coin" 
		on:keyup={({target: {value}}) => searchCoin(value)}
		bind:this={ref}>
    <table class="table table-dark">
      <thead>
        <tr>
          {#each titles as title}
			<th>
				{title}
			</th>
		  {/each}
        </tr>
      </thead>
      <tbody>
        {#each filteredCoins as coin, i}
          <tr>
			<td class="text-muted">
              {i + 1}
            </td>
            <td>
				<img src={coin.image} alt={coin.name} style="width: 2rem;" class="image-fluid me-2">
				<span>
					 {coin.name}
				</span>
				<span class="text-muted text-uppercase ms-2">
					{coin.symbol}
				</span>
            </td>
			<td>
				$ {coin.current_price.toLocaleString()}
			</td>
			<td class={coin.price_change_percentage_24h > 0 ? "text-success" : "text-danger"}>
				{coin.price_change_percentage_24h} %
			</td>
			<td>
				$ {coin.total_volume.toLocaleString()}
			</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>

<style>

</style>
