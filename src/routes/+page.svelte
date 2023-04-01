<script lang="ts">
	let hours: string[][] = [['', '']];
	let total = '0h00min';

	$: {
		const [lastStart, lastEnd] = hours[hours.length - 1];
		if (lastStart !== '' && lastEnd !== '') hours.push(['', '']);
	}

	const getTotal = (time: string[][]) => {
		const asMinutes = time.reduce((minutes, [start, end]) => {
			const [startHour, startMinute] = start ? start.split(':').map(Number) : [0, 0];
			const [endHour, endMinute] = end ? end.split(':').map(Number) : [0, 0];
			const startTotal = startHour * 60 + startMinute;
			const endTotal = endHour * 60 + endMinute;
			return minutes + (endTotal - startTotal);
		}, 0);

		const hours = Math.floor(asMinutes / 60);
		const minutes = asMinutes % 60;

		return `${hours}h${minutes}min`;
	};

	$: {
		const nonEmpty = hours.filter(([start, end]) => start !== '' || end !== '');

		if (nonEmpty.every(([start, end]) => start !== '' && end !== '')) {
			total = getTotal(nonEmpty);
		}
	}
</script>

<header>
	<h1>Hours Calculator</h1>
</header>

<div class="container">
	<article>
		{#each hours as [start, end], index}
			<div class="grid">
				<label for="start-{index}">
					Start
					<input type="time" id="start-{index}" bind:value={start} />
				</label>
				<label for="end-{index}">
					End
					<input type="time" id="end-{index}" bind:value={end} />
				</label>
			</div>
		{/each}
		<hr />
		<h2><center>{total}</center></h2>
	</article>
</div>

<footer>
	<small>
		Built with
		<a href="https://svelte.dev" target="_blank">Svelte</a> and
		<a href="https://picocss.com" target="_blank">Pico</a>
		•
		<a href="https://github.com/rodrigoaddor/hours.rodr.dev">Source code</a>
	</small>
</footer>
