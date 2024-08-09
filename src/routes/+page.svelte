<script>
	let enDate = '';
	let nsDate = '';
	let isoRef = '';
	let enRef = '';
	let copySuccess = false; // State to show/hide success message

	function handleSubmit(event) {
		event.preventDefault();
		enDate = '';
		nsDate = '';
		isoRef = '';
		enRef = '';
	}

	function copyToClipboard() {
		const textToCopy = `
			${enDate} ble internasjonal standard ${isoRef} adoptert som europeisk standard ${enRef}.\n\n${nsDate} ble europeisk standard ${enRef} fastsatt som Norsk Standard NS-${isoRef}. Engelsk versjon ble utgitt ${nsDate}.\n\nVed eventuell uoverensstemmelse, vil PDF-versjon legges til grunn.
		`.trim();
		// Copy the text to clipboard
		navigator.clipboard.writeText(textToCopy.trim())
			.then(() => {
				copySuccess = true; // Show success message
				setTimeout(() => copySuccess = false, 2000); // Hide after 2 seconds
			})
			.catch(err => {
				console.error("Failed to copy text: ", err);
			});
	}
</script>

<h1 class="text-2xl">Lag et forord "NS-EN 2 - unntak 1"</h1>
<form on:submit={handleSubmit} class="flex flex-col justify-center gap-3">
	<div class="flex gap-3">
		<label class="flex flex-col">
			EN dato:
			<input type="text" bind:value={enDate} class="outline outline-2" />
		</label>
		<label class="flex flex-col">
			NS-ISO dato:
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
	<button on:click={copyToClipboard} class="mt-3 p-2 bg-blue-500 text-white rounded">
		Kopier til utklippstavle
	</button>
	{#if copySuccess}
		<p class="mt-2 text-green-500">Teksten er kopiert til utklippstavlen!</p>
	{/if}
{/if}
