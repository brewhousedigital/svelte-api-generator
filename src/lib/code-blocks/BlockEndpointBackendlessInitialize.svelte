<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Initialize Backendless API

{:else}

// Create Backendless endpoint
let endpoint = generateBackendlessRestAPI(
    process.env['BACKENDLESS_APP_ID'],
    process.env['BACKENDLESS_API_KEY_REST']
);

{/if}