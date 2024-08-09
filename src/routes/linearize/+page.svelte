<script>
	const { DOMParser, XMLSerializer } = require('xmldom');

	const xmlString = '<root><child>Text</child></root>';
	const doc = new DOMParser().parseFromString(xmlString, 'application/xml');

	// Manipulate the DOM
	const child = doc.getElementsByTagName('child')[0];
	child.textContent = 'Updated Text';

	// Serialize the DOM back to a string
	const serializer = new XMLSerializer();
	const updatedXML = serializer.serializeToString(doc);

	console.log(updatedXML);
</script>
