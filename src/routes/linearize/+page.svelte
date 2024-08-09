<script>
	import { DOMParser, XMLSerializer } from 'xmldom';

	let file = null;
	let errorMessage = null;
	let downloadUrl = null;

	function handleFileUpload(event) {
		const target = event.target;
		if (target.files && target.files.length > 0) {
			file = target.files[0];
			errorMessage = null;
			downloadUrl = null;
		}
	}

	function replaceLabelText(text) {
		// Define the replacements
		const replacements = {
			Table: 'Tabell',
			Figure: 'Figur',
			Annex: 'Tillegg',
			Formula: 'Formel',
			NOTE: 'MERKNAD'
		};

		// Replace the first word if it matches any of the keys
		for (let key in replacements) {
			if (text.startsWith(key)) {
				return text.replace(key, replacements[key]);
			}
		}

		// Return the original text if no match was found
		return text;
	}

	async function processFile() {
		if (!file) {
			errorMessage = 'Please select an XML file.';
			return;
		}

		try {
			const text = await file.text();

			// Parse the XML string into a DOM
			const doc = new DOMParser().parseFromString(text, 'application/xml');

			// Iterate through elements and modify the labels
			const labels = doc.getElementsByTagName('label');
			for (let i = 0; i < labels.length; i++) {
				const label = labels[i];
				if (label.textContent) {
					label.textContent = replaceLabelText(label.textContent.trim());
				}
			}

			// Linearize the XML by removing unnecessary spaces and newlines
			const serializer = new XMLSerializer();
			const linearizedXml = serializer.serializeToString(doc).replace(/>\s+</g, '><');

			// Create a Blob with the new XML
			const blob = new Blob([linearizedXml]);

			// Create a download URL
			downloadUrl = URL.createObjectURL(blob);
		} catch (error) {
			errorMessage = 'An error occurred while processing the file.';
			console.error(error);
		}
	}
</script>

<main class="flex flex-col gap-3">
	<h1 class="text-2xl">Upload and Linearize XML File</h1>

	<input type="file" accept=".xml" on:change={handleFileUpload} />

	<button on:click={processFile}>Process File</button>

	{#if errorMessage}
		<p style="color: red;">{errorMessage}</p>
	{/if}

	{#if downloadUrl}
		<a href={downloadUrl} download="linearized.xml">Download Linearized XML</a>
	{/if}
</main>
