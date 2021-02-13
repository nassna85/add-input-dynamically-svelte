<script>
    export let name;
    export let data;
    export let value;
</script>

<div class="form-group">
    <select {name} bind:value on:change>
        <option value="none">--Please choice a country--</option>
        {#each data as item}
            <option value={item.value}>{item.name}</option>
        {/each}
    </select>
</div>

<style>
    select {
        margin-right: .5rem;
    }
</style>