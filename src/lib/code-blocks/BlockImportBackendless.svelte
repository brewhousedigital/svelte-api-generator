<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Import Backendless

{:else}

// Creates a backendless API point using your environment keys
import &lbrace;generateBackendlessRestAPI} from "$lib/js/generateBackendlessRestAPI";

{/if}