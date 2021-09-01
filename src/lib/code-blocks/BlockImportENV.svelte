<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Import Environment Variables

{:else}

// Import secret keys
import dotenv from 'dotenv';
dotenv.config();

{/if}