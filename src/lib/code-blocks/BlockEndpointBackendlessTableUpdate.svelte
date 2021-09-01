<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Backendless Table Update

{:else}

// Update a record/records for a specific table
let updateItem;
try &lbrace;
    // Backendless PUT requests require an objectId to be passed in the URL
    let table = `/data/Users/$&lbrace;data.objectId}`;
    let api = endpoint + table;

    updateItem = await fetch(api, &lbrace;
        method: 'PUT',
        headers: &lbrace;'Content-Type': 'application/json'},
        body: JSON.stringify(&lbrace;
            columnName: data.yourData,
        })
    });

    // PUT requests return a single object
    updateItem = await updateItem.json();
}
catch (error) &lbrace;
    console.log("---- unable to update object in backendless ----")
    console.log("endpoint", `/data/Users/$&lbrace;data.objectId}`)
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