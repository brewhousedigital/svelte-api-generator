<script>
    export let view = "normal";
</script>


{#if view === "normal"}

    Data Check

{:else}

if(true) &lbrace;
    let message = "Missing value that is required for this endpoint.";

    console.log("---- This condition failed ----")
    console.log(message)
    console.log("---- end error ----")

    return &lbrace;
        statusCode, headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: message
        })
    };
}

{/if}