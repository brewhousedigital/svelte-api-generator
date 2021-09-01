<script>
    // Notes
    // Use this to encode the code blocks
    // https://www.toptal.com/designers/htmlarrows/punctuation/left-brace/


    // Load components
    import Checkbox from "$lib/components/Checkbox.svelte";
    import Button from "$lib/components/Button.svelte";

    // Import helper functions
    import {afterUpdate, onMount} from "svelte";
    import {partyJS} from "$lib/party";

    // Load import things
    import BlockImportENV from "$lib/code-blocks/BlockImportENV.svelte";
    import BlockImportBackendless from "$lib/code-blocks/BlockImportBackendless.svelte";
    import BlockImportStripe from "$lib/code-blocks/BlockImportStripe.svelte";

    // Load API components
    import BlockEndpointBackendlessInitialize from "$lib/code-blocks/BlockEndpointBackendlessInitialize.svelte";
    import BlockEndpointBackendlessTableUpdate from "$lib/code-blocks/BlockEndpointBackendlessTableUpdate.svelte";
    import BlockEndpointBackendlessTableCreate from "$lib/code-blocks/BlockEndpointBackendlessTableCreate.svelte";

    import BlockEndpointStripeCreateLineItems from "$lib/code-blocks/BlockEndpointStripeCreateLineItems.svelte";
    import BlockEndpointStripeCreateCheckoutSession from "$lib/code-blocks/BlockEndpointStripeCreateCheckoutSession.svelte";
    import BlockEndpointStripeCreateCustomer from "$lib/code-blocks/BlockEndpointStripeCreateCustomer.svelte";
    import BlockEndpointStripeConfirmCheckoutPayment from "$lib/code-blocks/BlockEndpointStripeConfirmCheckoutPayment.svelte";

    // Switch statement can return components since they cant be referenced by strings
    function returnComponent(name) {
        switch (name) {
            // Import Components
            case "BlockImportENV": return BlockImportENV;
            case "BlockImportBackendless": return BlockImportBackendless;
            case "BlockImportStripe": return BlockImportStripe;

            // API components
            case "BlockEndpointBackendlessInitialize": return BlockEndpointBackendlessInitialize;
            case "BlockEndpointBackendlessTableUpdate": return BlockEndpointBackendlessTableUpdate;
            case "BlockEndpointBackendlessTableCreate": return BlockEndpointBackendlessTableCreate;

            case "BlockEndpointStripeCreateLineItems": return BlockEndpointStripeCreateLineItems;
            case "BlockEndpointStripeCreateCheckoutSession": return BlockEndpointStripeCreateCheckoutSession;
            case "BlockEndpointStripeCreateCustomer": return BlockEndpointStripeCreateCustomer;
            case "BlockEndpointStripeConfirmCheckoutPayment": return BlockEndpointStripeConfirmCheckoutPayment;
        }
    }

    // Set the template
    let templateImportItems = [];

    let templateEndpointItems = [];

    let includeExportedJson = false;
    let exportedJSON = "";
    let importError = "";

    onMount(() => {
        Prism.highlightAll();

        // Activate confetti!
        partyJS();
    })

    afterUpdate(() => {
        Prism.highlightAll();

        exportedJSON = JSON.stringify({
            imports: [...templateImportItems],
            content: [...templateEndpointItems],
        });
    })

    // https://gomakethings.com/how-to-reorder-an-item-in-an-array-with-vanilla-js/
    function moveInArray(array, from, to) {
        // Delete the item from it's current position
        let item = array.splice(from, 1);

        // Move the item to its new position
        array.splice(to, 0, item[0]);
    }

    function handleMoveItem(e) {
        let thisEl = e.target;

        // Click events can trigger on the <i> tag
        if(thisEl.tagName === "I") thisEl = thisEl.parentNode;

        // This is how we know where to move the item
        let index = parseInt(thisEl.getAttribute("data-index"));
        let newIndex;

        // Create a new ID for Svelte
        let newRandomID = "n" + Math.floor(Math.random() * 99999999999);
        let id = thisEl.getAttribute("data-move-id") + "";

        // Go up or down depending on button pressed
        let direction = thisEl.getAttribute("data-move-direction");
        direction === "up" ? newIndex = index - 1 : newIndex = index + 1;

        // Copy the array
        let temporaryList = [...templateEndpointItems];

        // Svelte keys can get confused so regenerate the key for Svelte to rebuild the list
        temporaryList = temporaryList.map(item => {
            if(item.id === id) {item.id = newRandomID}
            return item;
        });

        // Add the item to the correct spot
        moveInArray(temporaryList, index, newIndex);

        // Rebuild the list
        templateEndpointItems = [...temporaryList];
    }

    function handleDeleteItem(e) {
        let thisEl = e.target;

        // Click events can trigger on the <i> tag
        if(thisEl.tagName === "I") thisEl = thisEl.parentNode;

        let id = thisEl.getAttribute("data-delete-id");

        templateEndpointItems = templateEndpointItems.filter(item => item.id !== id);
    }

    function handleCopyCode(e) {
        let codeToBeCopied = document.getElementById("copy-code");
        let codeBox = document.getElementById("code-box")

        codeBox.value = codeToBeCopied.textContent;

        codeBox.select();

        document.execCommand("copy")

        // Confetti!
        party.confetti(e.target);
    }

    function handleCopyCodeExportOnly() {
        let codeBox = document.getElementById("code-box-export-only")

        codeBox.select();

        document.execCommand("copy");

        // Confetti!
        party.confetti(document.getElementById("export-json-btn"));

        // Now dismiss the modal
        let closeBtn = document.querySelector("#export-json-modal .btn-close").click();
    }

    function handleCodeImport() {
        // Reset error message
        importError = "";

        let json = document.getElementById("import-json-box").value;

        try {
            json = JSON.parse(json);

            templateImportItems = [...json['imports']];
            templateEndpointItems = [...json['content']];

            templateImportItems.forEach(item => {
                document.getElementById(item.component).checked = true;
            })

            // Confetti!
            party.confetti(document.getElementById("import-json-btn"));

            // Now dismiss the modal
            let closeBtn = document.querySelector("#import-json-modal .btn-close").click();
        }
        catch(error) {
            importError = "This is not valid JSON";
        }
    }

</script>


<svelte:head>

</svelte:head>


<div class="container-fluid px-5">
    <h1 class="mb-5">API Generator for Svelte Endpoints</h1>
    <div class="row">
        <nav class="col">
            <button class="btn btn-outline-primary" type="button" data-bs-toggle="modal" data-bs-target="#export-json-modal">Export</button>

            <button class="btn btn-outline-primary" type="button" data-bs-toggle="modal" data-bs-target="#import-json-modal">Import</button>

            <hr>

            <div class="form-check mb-4">
                <input class="form-check-input"
                       type="checkbox"
                       id="include-exported-json"
                       name="include-exported-json"
                       bind:checked={includeExportedJson}>
                <label class="form-check-label" for="include-exported-json">Include Exported JSON</label>
            </div>

            <Checkbox bind:array={templateImportItems} componentName="BlockImportENV" text="Environment Variables" />
            <Checkbox bind:array={templateImportItems} componentName="BlockImportBackendless" text="Backendless API" />
            <Checkbox bind:array={templateImportItems} componentName="BlockImportStripe" text="Stripe API" />

            <hr>

            <Button bind:array={templateEndpointItems} componentName="BlockEndpointBackendlessInitialize" text="Initialize Backendless" />
            <Button bind:array={templateEndpointItems} componentName="BlockEndpointBackendlessTableCreate" text="Backendless Create Item" />
            <Button bind:array={templateEndpointItems} componentName="BlockEndpointBackendlessTableUpdate" text="Backendless Update Item" />

            <Button bind:array={templateEndpointItems} componentName="BlockEndpointStripeCreateCustomer" text="New Stripe Customer" />
            <Button bind:array={templateEndpointItems} componentName="BlockEndpointStripeCreateLineItems" text="Stripe Line Items" />
            <Button bind:array={templateEndpointItems} componentName="BlockEndpointStripeCreateCheckoutSession" text="Stripe Checkout" />
            <Button bind:array={templateEndpointItems} componentName="BlockEndpointStripeConfirmCheckoutPayment" text="Stripe Confirm Payment" />

        </nav><!-- end col -->

        <main class="col">
            <div class="card card-body">
                {#each templateImportItems as item}
                    <p><svelte:component this={returnComponent(item.component)} view="normal" /></p>
                {/each}

                {#if templateImportItems.length > 0 && templateEndpointItems.length > 0}<hr>{/if}


                {#each templateEndpointItems as item, index (item.id)}
                    <div class="d-flex justify-content-between endpointItem">
                        <div class="col-auto"><svelte:component this={returnComponent(item.component)} view="normal" /></div>
                        <div class="col-auto">
                            <button class="btn btn-sm" on:click={handleMoveItem} data-index={index} data-move-direction="up" data-move-id={item.id}><i class="bi bi-arrow-up-circle"></i></button>
                            <button class="btn btn-sm" on:click={handleMoveItem} data-index={index} data-move-direction="down" data-move-id={item.id}><i class="bi bi-arrow-down-circle"></i></button>
                            <button class="btn btn-sm" on:click={handleDeleteItem} data-delete-id={item.id}><i class="bi bi-trash"></i></button>
                        </div>
                    </div>
                {/each}
            </div><!-- end card -->
        </main><!-- end col -->

        <aside class="col">
            <div class="card card-body p-5 position-relative">
                <div id="copy-code">

                    {#if includeExportedJson}
                <pre class="text-muted"><code>// Endpoint Export Code <br>// https://svelte-endpoint-generator.netlify.app/</code></pre>
                <pre class="text-muted" style="max-width: 938px;white-space: normal;"><code>/* {exportedJSON} */{`

`}</code></pre>{/if}

                {#each templateImportItems as item (item.id)}<pre class="language-javascript"><code><svelte:component this={returnComponent(item.component)} view="epic" />{`

`}</code></pre>{/each}




                {#if templateEndpointItems.length > 0}
    <pre class="language-javascript pt-3"><code>
// Set default response codes
const statusCode = 200;
const headers = &lbrace;
    "Access-Control-Allow-Origin" : "*",
    "Access-Control-Allow-Headers": "Content-Type"
};{`

`}</code></pre>

                    <pre class="language-javascript"><code>export async function post(request) &lbrace;{`

`}</code></pre>

                    <pre class="language-javascript ps-5"><code>const data = request.body;{`

`}</code></pre>

                    <pre class="language-javascript ps-5"><code>
// Make sure we have all required fields. Otherwise, get outta here.
if (
    !data.firstname ||
    !data.lastname
) &lbrace;
    let message;

    if(!data.firstname) message = "Example error for missing 'firstname' value.";
    if(!data.lastname) message = "Example error for missing 'lastname' value.";

    console.log("---- Missing POST data for API ----")
    console.log(message)
    console.log("---- end error ----")

    return &lbrace;
        statusCode, headers,
        body: JSON.stringify(&lbrace;
            status: "failed",
            message: message
        })
    };
}{`

`}</code></pre>

                    {#each templateEndpointItems as item (item.id)}
                        <pre class="language-javascript ps-5"><code><svelte:component this={returnComponent(item.component)} view="epic" />{`

`}</code></pre>
                    {/each}

<pre class="language-javascript ps-5"><code>
// Return the necessary data back to the client
return &lbrace;
    statusCode, headers,
    body: JSON.stringify(&lbrace;
        status: "success",
        data: dataGoesHere,
        message: "Custom message!"
    })
};{`

`}</code></pre>

<pre class="language-javascript"><code>}{`

`}</code></pre>


                {/if}
                </div><!-- end copy code function -->

                <textarea id="code-box"></textarea>

                <button id="copy-btn" on:click={handleCopyCode} class="btn btn-primary">Copy</button>
            </div><!-- end card -->
        </aside><!-- end col -->
    </div><!-- end row -->
</div><!-- end container -->


<!-- Export JSON Modal -->
<div class="modal fade" id="export-json-modal" tabindex="-1" aria-labelledby="export-json-modal-label" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="export-json-modal-label">Export JSON</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>

            <div class="modal-body">
                <pre style="white-space: normal;"><code>{exportedJSON}</code></pre>
                <textarea id="code-box-export-only">{exportedJSON}</textarea>
            </div>

            <div class="modal-footer">
                <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">Close</button>
                <button id="export-json-btn" type="button" class="btn btn-primary" on:click={handleCopyCodeExportOnly}>Copy</button>
            </div>
        </div>
    </div>
</div>


<!-- Import JSON Modal -->
<div class="modal fade" id="import-json-modal" tabindex="-1" aria-labelledby="import-json-modal-label" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="import-json-modal-label">Import Endpoint JSON</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>

            <div class="modal-body">
                <textarea class="form-control font-monospace" id="import-json-box" style="font-size: 13px;" rows="6"></textarea>

                {#if importError !== ""}
                    <div class="alert alert-warning mt-2" role="alert">{importError}</div>
                {/if}
            </div>

            <div class="modal-footer">
                <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">Close</button>
                <button id="import-json-btn" type="button" class="btn btn-primary" on:click={handleCodeImport}>Import</button>
            </div>
        </div>
    </div>
</div>




<style>
    nav {
        max-width: 250px
    }

    nav .btn {
        padding: 8px;
        width: 100%;
        margin-bottom: 8px;
        display: block;
    }

    main {
        width: calc(40% - (250px / 2));
    }

    main .endpointItem:first-of-type [data-move-direction='up'] {
        visibility: hidden;
    }

    main .endpointItem:last-of-type [data-move-direction='down']  {
        visibility: hidden;
    }

    aside {
        width: calc(60% - (250px / 2));
    }

    aside .card {
        background-color: #272822;
        overflow: scroll;
        height: calc(100vh - 100px);
    }

    aside .card::-webkit-scrollbar {
        width: 4px;
        height: 6px;
        background-color: #000;
    }

    aside .card::-webkit-scrollbar-thumb {
        background-color: #2ECC71;
        border-radius: 2px;
    }

    aside .card::-webkit-scrollbar-track {
        background-color: #000;
    }

    aside pre {
        margin: 0!important;
        border-radius: 0!important;
        padding: 0 0 20px 0;
        overflow: visible;
    }

    #copy-btn {
        position: absolute;
        right: 0;
        top: 0;
        z-index: 10;
    }

    #code-box, #code-box-export-only {
        opacity: 0;
        position: absolute;
        height: 0;
        width: 0;
    }
</style>