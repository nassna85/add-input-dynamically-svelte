<script>
    export let id;
    export let label;
    export let type = "text";
    export let name;
    export let placeholder;
    export let value;

    const typeAction = node => {
        node.type = type;
    }
</script>

<div class="form-group">
    {#if label}
        <label for={id}>{label}</label>
    {/if}
    <input use:typeAction {id} {name} {placeholder} bind:value on:keyup />
</div>

<style>
    input {
        margin-right: .5rem;
    }
</style>