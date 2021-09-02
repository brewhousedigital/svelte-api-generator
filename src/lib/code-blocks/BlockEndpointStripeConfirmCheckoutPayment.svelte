<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Create Stripe Checkout Session

{:else}

// Retrieve a Stripe Checkout Session to confirm the "payment_status"
// https://stripe.com/docs/api/checkout/sessions/retrieve?lang=node
let searchForCheckoutSession;
try &lbrace;searchForCheckoutSession = await stripe.checkout.sessions.retrieve(data.checkoutID)}
catch (error) &lbrace;
    console.log("---- Unable to retrieve Stripe checkout session ----")
    console.log(error)
    console.log("---- end error ----")
}

// See if the Stripe checkout was valid
if(searchForCheckoutSession === undefined) &lbrace;
    console.log("---- Stripe checkout session was not valid ----")
    console.log(data.code)
    console.log("---- end error ----")

    return &lbrace;
        statusCode, headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: "Unable to find account"
        })
    };
}

// See if the Stripe checkout is set to "paid"
if(searchForCheckoutSession.payment_status !== "paid") &lbrace;
    console.log("---- Stripe checkout session was not paid ----")
    console.log(data.code)
    console.log("---- end error ----")

    return &lbrace;
        statusCode, headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: "This business builder invoice has not been paid for"
        })
    };
}

{/if}