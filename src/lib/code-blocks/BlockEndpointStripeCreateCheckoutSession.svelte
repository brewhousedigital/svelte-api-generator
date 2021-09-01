<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Create Stripe Checkout Session

{:else}

// Create stripe checkout session
// https://stripe.com/docs/api/checkout/sessions/create?lang=node
let stripeCheckoutSession;
try &lbrace;
    stripeCheckoutSession = await stripe.checkout.sessions.create(&lbrace;
        success_url: process.env['DOMAIN'] + '/SUCCESS-URL-GOES-HERE',
        cancel_url: process.env['DOMAIN'] + '/CANCEL-URL-GOES-HERE',
        payment_method_types: ['card'],

        // Pass the customer ID to Stripe to sync up the customer to their account
        customer: data.stripe_id,

        line_items: stripeLineItems,
        mode: 'subscription',
    });
}
catch (error) &lbrace;
    // Log all details to Netlify functions console
    console.log("---- Unable to create stripe session ----")
    console.log(error)
    console.log("---- end error ----")

    // Send the error back to the user
    return &lbrace;
        statusCode, headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: error.message
        })
    };
}

// Send user to Stripe Session
const stripeCheckoutId = stripeCheckoutSession.id;

{/if}