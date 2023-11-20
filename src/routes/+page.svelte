<script>
    import Password from "$lib/Password/Password.svelte";
    let password = "";
    let placeholder = "Password";
    let validated = false;
    let validate = {
        length: { min: 12 },
        uppercase: { min: 1 },
        lowercase: { min: 1 },
        numbers: { min: 1 },
        special: { min: 1 }
    }
    let showValidations = true;
    let showIcon = true;
    let showHideToggle = true;
    let suggestPassword = true;

    function handlePasswordEvent(e) {
        console.log(e.detail);
        /*
            returns
            {
                password: "",
                validate: {},
                validated: boolean
            }
        */
    }

</script>

<Password bind:password bind:validated bind:validate {showValidations} {showIcon} {showHideToggle} {placeholder} {suggestPassword} on:passwordCheck={handlePasswordEvent}/>
<p>Password: {password}</p>
<p>Validated: {validated}</p>
<pre>Validate: {JSON.stringify(validate,null,2)}</pre>