<script lang="ts">
	import { writable, derived } from 'svelte/store';

	const cpuPrice = 0.88;
	const ramPrice = 2.4;
	const ssdPrice = 0.3;

	const discount = writable(10); // in percent

	const cpuAmount = writable(1);
	const ramAmount = writable(1);
	const ssdAmount = writable(1);

	const price = derived(
		[discount, cpuAmount, ramAmount, ssdAmount],
		([$discount, $cpuAmount, $ramAmount, $ssdAmount]) => {
			const priceWithoutDiscount = cpuPrice * $cpuAmount + ramPrice * $ramAmount + ssdPrice * $ssdAmount;
			const price = priceWithoutDiscount - (priceWithoutDiscount * $discount) / 100;

			return [Number(price.toFixed(2)), Number(priceWithoutDiscount.toFixed(2))];
		}
	);

	function toMonthPrice(price: number): string {
		return (price * 30).toFixed(2);
	}
</script>

<div class="flex">
	<div class="hero">
		<div class="hero-content text-center">
			<div class="max-w-md">
				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">ЦПУ</span>
						<span class="label-text-alt">{cpuPrice}/1 ядро</span>
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
						<span class="label-text">ОЗУ</span>
						<span class="label-text-alt">{ramPrice}/1гб</span>
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
						<span class="label-text">Диск</span>
						<span class="label-text-alt">{ssdPrice}/1гб</span>
					</div>
					<input
						type="number"
						step="0.5"
						bind:value={$ssdAmount}
						class="input input-bordered w-full max-w-xs"
					/>
				</label>

				<label class="form-control w-full max-w-xs">
					<div class="label">
						<span class="label-text">Скидка (в %)</span>
					</div>
					<input
						type="number"
						bind:value={$discount}
						class="input input-bordered w-full max-w-xs"
						disabled={true}
					/>
				</label>
			</div>
		</div>
	</div>
</div>

<div class="hero">
	<div class="hero-content text-center">
		<div class="max-w-md">
			<h1 class="text-4xl font-bold mb-3">Результат</h1>
			<p>{$price[0]}/день ({$price[1]}/день без скидки)</p>
			<p>{toMonthPrice($price[0])}/месяц ({toMonthPrice($price[1])}/месяц без скидки)</p>
		</div>
	</div>
</div>
