<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Create Stripe Line Items

{:else}

let stripeLineItems = [
    // Price ID example
    &lbrace;price: process.env['STRIPE_PRICE_ACCOUNT_EXECUTIVE'], quantity: 1},

    // Inline charge example
    // https://stripe.com/docs/api/checkout/sessions/create#create_checkout_session-line_items-price_data-recurring
    &lbrace;
        price_data: &lbrace;
            currency: "usd",
            product_data: &lbrace;name: "Processing Fee"},
            unit_amount: 15100,
            recurring: &lbrace;interval: "month", interval_count: 1}
        },
        quantity: 1
    }
]

{/if}