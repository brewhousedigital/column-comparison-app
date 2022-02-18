<script>
    export let row = [];
</script>

<div class="d-flex align-items-center">
    <div class="id-block font-monospace small fw-bold" >{row[3] ? row[3] : ""}</div>
    <div class="row-block font-monospace small color-blue fw-bold">row {row[0]}:</div>
    {#if row[1] === "__EMPTY__"}
        <div class="text-block color-green fst-italic small">BOTH EMPTY</div>
    {:else}
        <div class="text-block">{row[1]}</div>
    {/if}

</div>


<style>
    .id-block {min-width: 105px}
    .row-block {min-width: 100px}
    .text-block {width: 100%;}

    .font-monospace {font-family: monospace}
    .color-blue {color: dodgerblue}
    .color-green {color: darkolivegreen}
</style>