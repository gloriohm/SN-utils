<script>
	let enDate = '';
	let nsDate = '';
	let isoRef = '';
	let enRef = '';

	function handleSubmit(event) {
		event.preventDefault();
		enDate = '';
		nsDate = '';
		isoRef = '';
		enRef = '';
	}
</script>

<h1 class="text-2xl">Lag et forord</h1>
<form on:submit={handleSubmit} class="flex flex-col justify-center gap-3">
	<div class="flex gap-3">
		<label class="flex flex-col">
			EN dato:
			<input type="text" bind:value={enDate} class="outline outline-2 bg-green-500" />
		</label>
		<label class="flex flex-col">
			NS dato:
			<input type="text" bind:value={nsDate} class="outline outline-2" />
		</label>
	</div>
	<div class="flex gap-3">
		<label class="flex flex-col">
			ISO referanse:
			<input type="text" bind:value={isoRef} class="outline outline-2" />
		</label>
		<label class="flex flex-col">
			EN referanse:
			<input type="text" bind:value={enRef} class="outline outline-2" />
		</label>
	</div>
	<button type="submit" class="my-5 outline outline-2 bg-slate-500 max-w-36 py-2">
		Tøm skjema
	</button>
</form>


{#if enDate && nsDate && isoRef && enRef}
	<div class="flex flex-col gap-3 bg-slate-300 outline outline-2 p-3">
		<p>
			{enDate} ble internasjonal standard {isoRef} adoptert som europeisk standard {enRef}.
		</p>
		<p>
			{nsDate} ble europeisk standard {enRef} fastsatt som Norsk Standard NS-{isoRef}. Engelsk
			versjon ble utgitt {nsDate}.
		</p>
		<p>Ved eventuell uoverensstemmelse, vil PDF-versjon legges til grunn.</p>
	</div>
{/if}
