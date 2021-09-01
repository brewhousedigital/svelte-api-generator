<script>
    export let array;
    export let componentName;
    export let text = "Missing Name";

    function handleRadio(e) {
        let randomId = "n" + Math.floor(Math.random() * 99999999999);

        if(e.target.checked) {
            // Checked
            array = [...array, {
                component: componentName,
                id: randomId
            }]
        } else {
            // Not checked
            array = array.filter(item => item.component !== componentName)
        }
    }
</script>


<div class="form-check mb-4">
    <input class="form-check-input"
           type="checkbox"
           id={componentName}
           name={componentName}
           value={componentName}
           on:change={handleRadio}>
    <label class="form-check-label" for={componentName}>{text}</label>
</div>