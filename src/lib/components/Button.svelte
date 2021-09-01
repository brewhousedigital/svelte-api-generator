<script>
    export let array;
    export let componentName;
    export let text = "Missing Name";

    function handleButton(e) {
        let randomId = "n" + Math.floor(Math.random() * 99999999999);

        array = [...array, {
            component: componentName,
            id: randomId
        }]
    }
</script>

<button class="btn btn-light mb-4 w-100 text-start" on:click={handleButton}>
    <i class="bi bi-plus-circle"></i> {text}
</button>