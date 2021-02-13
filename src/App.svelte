<script>
	import TextField from "./components/UI/TextField.svelte";
	import SelectField from "./components/UI/SelectField.svelte";
	import { localEntities } from "./data/localEntities";

	let partner = {
		firstname: "",
		lastname: "",
		localEntities: []
	};

	let errorLocalEntities = "";

	let localEntitiesData = [{localCountry: "", localVatID: ""}];

	const handleSubmit = e => {
		e.preventDefault();
		errorLocalEntities = "";
		partner.localEntities = localEntitiesData;
		if(partner.localEntities.length > 0) {
			partner.localEntities.forEach(local => {
				if(local.localCountry === "" || local.localCountry === "none" || local.localVatID === "") {
					return errorLocalEntities = "Please fill al fields about local entities !";
				}
			})
		}
		console.log(partner);
	}
	const handleAddEntity = () => {
		errorLocalEntities = "";
		enableAddLocalEntitiesButton = false;
		localEntitiesData = [...localEntitiesData, {localCountry: "", localVatID: ""}];
	}

	const handleRemoveEntity = index => {
		errorLocalEntities = "";
		enableToggleAddEntities();
		const values = [...localEntitiesData];
		values.splice(index, 1);
		localEntitiesData = values;
	}

	$: enableAddLocalEntitiesButton = false;

	const enableToggleAddEntities = () => {
		if (localEntitiesData.length > 0) {
			localEntitiesData.forEach(local => {
				enableAddLocalEntitiesButton = local.localCountry !== "none" && local.localVatID !== "";
			})
		} else {
			enableAddLocalEntitiesButton = true;
		}
	}
	$:console.log(enableAddLocalEntitiesButton);

</script>

<main>
	<h1>Add input dynamically</h1>

	<form>
		<TextField
			placeholder="Prénom"
			name="firstname"
			label="Votre prénom"
			bind:value={partner.firstname}
		/>
		<TextField
			placeholder="Nom"
			name="lastname"
			label="Votre nom"
			bind:value={partner.lastname}
		/>
		{#each localEntitiesData as local, i}
			<div class="inputs-local-entities">
				<SelectField
					name="localCountry"
					data={localEntities}
					bind:value={local.localCountry}
					on:change={enableToggleAddEntities}
				/>
				<TextField
					placeholder="VatID"
					name="localVatID"
					bind:value={local.localVatID}
					on:keyup={enableToggleAddEntities}
            />
            <button type="button" on:click={() => handleRemoveEntity(i)}>Remove</button>
        </div>
    {/each}

    {#if errorLocalEntities}
        <p>{errorLocalEntities}</p>
    {/if}

	{#if enableAddLocalEntitiesButton}
    	<button type="button" on:click={() => handleAddEntity()}>Add new entity</button>
	{/if}

    <div>
        <button on:click={handleSubmit} type="submit">Submit</button>
    </div>
</form>
</main>

<style>
	main {
		padding: 1em;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 3em;
		font-weight: 100;
	}
	.inputs-local-entities {
		display: flex;
	}
</style>