<script>
    import Object from './Object.svelte';
    export let json;
    export let name;
    export let hidden = true;
	let data = [];
	let names = [];
    if(typeof json == "object"){
        for(let i in json){
            names.push(i);
            data.push(json[i]);
        }
    }
    else{
        let j = 0;
        for(let i of json){
            names.push(j);
            data.push(i);
            j++;
        }
    }
    const hide = () => {
        hidden = !hidden;
        console.log(hidden);
    }
</script>
{#if json}
    <button on:click = {hide}>{hidden? "+":"-"}</button>
    {#if name}
        <span>{name}</span>
    {/if}
    <br>
    <span id = "content" hidden = {hidden}>
        {#each data as obj, i}
            {#if typeof obj != "object" && typeof obj != "array"}
                <label for = {names[i]}>{names[i]}</label> 
                {#if typeof obj == "boolean"}
                    <input bind:checked = {json[names[i]]} type = "checkbox" id = {names[i]}><br>
                {/if}
                {#if typeof obj == "number"}
                    <input type = "number" id = {names[i]} bind:value = {json[names[i]]} ><br>
                {/if}
                {#if typeof obj == "string"}
                    <input type = "text" id = {names[i]} bind:value = {json[names[i]]} ><br>
                {/if}
            {/if}
            {#if typeof obj == "object" || typeof obj == "array"}
                <Object name = {names[i]} json = {json[names[i]]}></Object>
            {/if}
        {/each}
    </span>
{/if}