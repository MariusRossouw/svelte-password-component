<script>
    import Password from "$lib/Password/Password.svelte";
    let password = "";
    let placeholder = "Password";
    let validated = false;
    let validate = {
        length: { cnt: 12 },
        uppercase: { cnt: 1 },
        lowercase: { cnt: 1 },
        numbers: { cnt: 1 },
        special: { cnt: 1 }
    }
    let showValidations = true;
    let showIcon = true;
    let showHideToggle = true;

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

<Password {placeholder} bind:password bind:validated bind:validate {showValidations} {showIcon} {showHideToggle} on:passwordCheck={handlePasswordEvent}/>
<p>Password: {password}</p>
<p>Validated: {validated}</p>
<p>Validate: {JSON.stringify(validate,null,2)}</p>