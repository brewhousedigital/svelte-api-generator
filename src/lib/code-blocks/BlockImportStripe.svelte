<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Import Stripe

{:else}

// Import Stripe Library
// https://github.com/stripe/stripe-node
// https://www.npmjs.com/package/stripe
import Stripe from 'stripe';
const stripe = new Stripe(process.env['STRIPE_SECRET_KEY']);

{/if}