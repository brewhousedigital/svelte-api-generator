<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Backendless Table Update

{:else}

// Update a record/records for a specific table using a WHERE clause
let updateItem;
try &lbrace;
    // Backendless Advanced PUT requests do not require an objectId
    let table = `/data/bulk/YOUR-TABLE-HERE`;

    let where = "?where=" + encodeURIComponent(`column_name1='$&lbrace;data.value}'`);
    let whereMultiple = "?where=" + encodeURIComponent(
        `column_name1='$&lbrace;data.value}' and column_name2='$&lbrace;data.value2}'`
    );

    let api = endpoint + table + where;

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
    console.log("endpoint", `/data/YOUR-TABLE-HERE`)
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