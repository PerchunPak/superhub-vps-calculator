<script lang="ts">
	import { writable, derived } from 'svelte/store';

	const cpuPrice = writable(0.88);
	const ramPrice = writable(2.4);
	const ssdPrice = writable(0.3);

	const discount = writable(10); // in percent

	const cpuAmount = writable(1);
	const ramAmount = writable(1);
	const ssdAmount = writable(1);

	const price = derived(
		[cpuPrice, ramPrice, ssdPrice, discount, cpuAmount, ramAmount, ssdAmount],
		([$cpuPrice, $ramPrice, $ssdPrice, $discount, $cpuAmount, $ramAmount, $ssdAmount]) => {
			const priceWithoutDiscount = $cpuPrice * $cpuAmount + $ramPrice * $ramAmount + $ssdPrice * $ssdAmount;
			const price = priceWithoutDiscount - (priceWithoutDiscount * $discount) / 100;

			return [Number(price.toFixed(2)), Number(priceWithoutDiscount.toFixed(2))];
		}
	);

	function toMonthPrice(price: number): number {
		return (price * 30).toFixed(2);
	}
</script>

<div class="flex">
	<div class="hero">
		<div class="hero-content text-center">
			<div class="max-w-md">
				<h1 class="text-4xl font-bold">Prices</h1>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">CPU</span>
					</div>
					<input
						type="number"
						step="0.01"
						bind:value={$cpuPrice}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">RAM</span>
					</div>
					<input
						type="number"
						step="0.01"
						bind:value={$ramPrice}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">SSD</span>
					</div>
					<input
						type="number"
						step="0.01"
						bind:value={$ssdPrice}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">Discount (in %)</span>
					</div>
					<input
						type="number"
						bind:value={$discount}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>
			</div>
		</div>
	</div>

	<div class="hero">
		<div class="hero-content text-center">
			<div class="max-w-md">
				<h1 class="text-4xl font-bold">Amounts</h1>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">CPU</span>
					</div>
					<input
						type="number"
						step="0.5"
						bind:value={$cpuAmount}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">RAM</span>
					</div>
					<input
						type="number"
						step="0.5"
						bind:value={$ramAmount}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">SSD</span>
					</div>
					<input
						type="number"
						step="0.5"
						bind:value={$ssdAmount}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>
			</div>
		</div>
	</div>
</div>

<div class="hero">
	<div class="hero-content text-center">
		<div class="max-w-md">
			<h1 class="text-4xl font-bold">Result price</h1>
			<p>{$price[0]}/day ({$price[1]}/day without discount)</p>
			<p>{toMonthPrice($price[0])}/month ({toMonthPrice($price[1])}/month without discount)</p>
		</div>
	</div>
</div>
