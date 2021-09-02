<script>
    export let view = "normal";
</script>


{#if view === "normal"}

Backendless Table Update

{:else}

// Retrieve items from database
let checkForItem;
try &lbrace;
    let table = "/data/YOUR-TABLE-HERE";
    // Where clause values need single quote marks, and the entire thing needs to be encoded
    let where = "?where=" + encodeURIComponent(`column_name1='$&lbrace;data.value}'`);
    let whereMultiple = "?where=" + encodeURIComponent(`column_name1='$&lbrace;data.value}' and column_name2='$&lbrace;data.value2}'`);

    // This returns specific columns
    let columns = "&property=column_1&property=column2";

    let api = endpoint + table;
    // let api = endpoint + table + where + columns;

    checkForItem = await fetch(api);
    checkForItem = await checkForItem.json();

    let json = checkForItem;

    // Check if item exists
    if(checkForItem.length === 0) &lbrace;
        console.log("---- Item was not found in the backendless table ----")
        console.log("api used", api)
        console.log(checkForItem)
        console.log("---- end error ----")

        return &lbrace;
            statusCode, headers,
            body: JSON.stringify(&lbrace;
                status: "failed",
                message: "Unable to find item"
            })
        };
    }

    // Remove from array if only one item is present
    if(checkForItem.length === 1) &lbrace;
        checkForItem = checkForItem[0];
    }
}
catch (error) &lbrace;
    console.log("---- Trying to build URL for user from the backendless table ----")
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