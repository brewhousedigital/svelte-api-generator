<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Backendless Table Update

{:else}

let createNewDatabaseEntry;
try &lbrace;
    let table = "/data/YOUR-TABLE-HERE";
    let api = endpoint + table;

    let environment = process.env['ENVIRONMENT'] === "production";

    createNewDatabaseEntry = await fetch(api, &lbrace;
        method: 'POST',
        headers: &lbrace;'Content-Type': 'application/json'},
        body: JSON.stringify(&lbrace;
            stripeCheckoutId: stripeCheckoutId,
            ownerId: data.userDetails.objectId,
            environment: environment
        })
    });

    createNewDatabaseEntry = await createNewDatabaseEntry.json();
}
catch (error) &lbrace;
    console.log("---- Unable to create new database entry ----")
    console.log(error)
    console.log("---- end error ----")

    return &lbrace;
        statusCode, headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: error.message
        })
    };
}

{/if}