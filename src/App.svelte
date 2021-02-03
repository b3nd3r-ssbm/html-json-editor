<script>
	import Object from './Object.svelte';
	let json;
	let data = [];
	let names = [];
	let name;
	const send = () => {
		download(name, JSON.stringify(json))
	}
	const update = async () => {
		json = JSON.parse(await document.getElementById("json").files[0].text());
		names = [];
		data = [];
		for(let i in json){
			names.push(i);
			data.push(json[i]);
		}
		name = document.getElementById("json").files[0].name;
	}
	const download = (filename, text) => {
		let element = document.createElement('a');
		element.setAttribute('href', 'data:text/json;charset=utf-8,' + encodeURIComponent(text));
		element.setAttribute('download', filename);

		element.style.display = 'none';
		document.body.appendChild(element);

		element.click();

		document.body.removeChild(element);
	}
</script>
{#if !json}
	<input type = "file" id = "json" accept = ".json">
	<button on:click = {update}>Load JSON!</button>
{/if}
{#if json}
	<Object json = {json} hidden = {false}></Object>
	<button on:click = {send}>Save!</button>
{/if}