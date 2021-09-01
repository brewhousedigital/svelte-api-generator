<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Create Stripe Customer

{:else}

// Create new stripe customer
let newStripeCustomer;

try &lbrace;
    // https://stripe.com/docs/api/customers/create
    newStripeCustomer = await stripe.customers.create(&lbrace;
        email: data.email,
        description: data.firstname + " " + data.lastname
    });
}
catch (error) &lbrace;
    // Log all details to Netlify functions console
    console.log("---- Unable to create stripe customer ----")
    console.log(error.message)
    console.log("---- end error ----")

    return &lbrace;
        statusCode: 424,
        headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: e.message
        })
    };
}

// Grab the ID out of the object
// https://stripe.com/docs/api/customers/object
const stripeCustomerId = newStripeCustomer.id;
const stripeEnvironment = newStripeCustomer.livemode; // Boolean

{/if}