<script>
    import { onMount } from "svelte";
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let placeholder = "";
    export let password = "";
    export let type = "password";
    export let validated = false;
    export let validate = {
        length: { min: 12 },
        uppercase: { min: 1 },
        lowercase: { min: 1 },
        numbers: { min: 1 },
        special: { min: 1 }
    };
    export let showValidations = true;
    export let showIcon = true;
    export let showHideToggle = true;
    export let suggestPassword = false;


    onMount(() => {
        if(suggestPassword) {
            generatePassword();
            eye();
        } else {
            handleInput({target:{value: ""}})
        }
    });

    function eye() {
        if (type == "password") {
            type = "text";
        } else {
            type = "password";
        }
    }

    function validateLength(input, requirement) {
        return input >= requirement;
    }

    const handleInput = (e) => {
        password = e.target.value;
        let validLength = 0;
        let reqLength = Object.keys(validate).length;
        if (validate["length"] && validate["length"].min) {
            validate["length"].input = {}
            validate["length"].input.length = password.length || 0;
            validate["length"].input.values = password || ""
            validate["length"].input.valid = validateLength(password.length, validate["length"].min) || false;
            if (validate["length"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["uppercase"] && validate["uppercase"].min) {
            let matchUpC = password.match(/[A-Z]/g) || [];
            validate["uppercase"].input = {}
            validate["uppercase"].input.length = matchUpC.length || 0
            validate["uppercase"].input.values = matchUpC || []
            validate["uppercase"].input.valid = validateLength(matchUpC.length, validate["uppercase"].min) || false
            if (validate["uppercase"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["lowercase"] && validate["lowercase"].min) {
            let matchLo = password.match(/[a-z]/g) || [];
            validate["lowercase"].input = {}
            validate["lowercase"].input.length = matchLo.length || 0
            validate["lowercase"].input.values = matchLo || []
            validate["lowercase"].input.valid = validateLength(
                matchLo.length,
                validate["lowercase"].min || false
            )
            if (validate["lowercase"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["numbers"] && validate["numbers"].min) {
            let matchNum = password.match(/[0-9]/g) || [];
            validate["numbers"].input = {}
            validate["numbers"].input.length = matchNum.length || 0
            validate["numbers"].input.values = matchNum || []
            validate["numbers"].input.valid = validateLength(matchNum.length, validate["numbers"].min) || false
            if (validate["numbers"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["special"] && validate["special"].min) {
            let matchSp =
                password.match(/[ @#$%~`!^&*()_+\-=\[\]{};':"\\|,.<>\/?]/g) ||
                [];
            validate["special"].input = {}
            validate["special"].input.length = matchSp.length || 0
            validate["special"].input.values = matchSp || []
            validate["special"].input.valid = validateLength(matchSp.length, validate["special"].min) || false
            if (validate["special"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (reqLength === validLength) {
            validated = true;
        } else {
            validated = false;
        }

        dispatch("passwordCheck", {
            password,
            validate,
            validated,
        });
    };

    function generatePassword() {
        let numberChars = "0123456789";
        let upperChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        let lowerChars = "abcdefghijklmnopqrstuvwxyz";
        let specialChars = " @#$%~`!^&*()_+\-=\[\]{};':\\|,.<>\/?" + '"';
        let allChars = numberChars + upperChars + lowerChars + specialChars;
        let initString = ""
        const shuffle = str => [...str].sort(()=>Math.random()-.5).join('');
        if(validate["uppercase"]) {
            let s = shuffle(upperChars);
            let ss = s.substring(0,validate["uppercase"].min);
            initString = initString + ss
        }
        if(validate["lowercase"]) {
            let s = shuffle(lowerChars);
            let ss = s.substring(0,validate["lowercase"].min);
            initString = initString + ss
        }
        if(validate["numbers"]) {
            let s = shuffle(numberChars);
            let ss = s.substring(0,validate["numbers"].min);
            initString = initString + ss
        }
        if(validate["special"]) {
            let s = shuffle(specialChars);
            let ss = s.substring(0,validate["special"].min);
            initString = initString + ss
        }
        if(validate["length"] && initString.length < validate["length"].min) {
            let s = shuffle(allChars);
            let randomnumber =  Math.random() * 30 | 20;
            let c = randomnumber - initString.length // can be validate["length"].min but that is the minimum requirement and not the best
            let ss = s.substring(0,c);
            initString = initString + ss
        }
        console.log("initString: ", initString)
        let f = shuffle(initString);
        console.log("final: ", f)
        handleInput({target:{value: f}})
    }
</script>

<form>
    <input
        class={showIcon ? "icon" : ""}
        {type}
        {password}
        {placeholder}
        value={password}
        on:input={handleInput}
        on:focus={handleInput}
        on:blur={handleInput}
    />
    {#if showHideToggle}
        <button class="eye" on:click={eye} />
    {/if}
</form>

{#if showValidations}
    {#if validate["uppercase"] && validate["uppercase"].input}
        <p class={validate["uppercase"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["uppercase"].min}
            uppercase charanters
        </p>
    {/if}
    {#if validate["lowercase"] && validate["lowercase"].input}
        <p class={validate["lowercase"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["lowercase"].min}
            lowercase charanters
        </p>
    {/if}
    {#if validate["special"] && validate["special"].input}
        <p class={validate["special"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["special"].min}
            special charanters
        </p>
    {/if}
    {#if validate["numbers"] && validate["numbers"].input}
        <p class={validate["numbers"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["numbers"].min}
            numbers
        </p>
    {/if}
    {#if validate["length"] && validate["length"].input}
        <p class={validate["length"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["length"].min} characters in total
        </p>
    {/if}
{/if}

<style>
    form {
        /* This bit sets up the horizontal layout */
        display: flex;
        flex-direction: row;

        /* This bit draws the box around it */
        border: var(--passwordFormBorder, 1px solid grey);

        /* I've used padding so you can see the edges of the elements. */
        padding: var(--passwordFormPadding, 1px);
    }

    input {
        /* Tell the input to use all the available space */
        flex-grow: 2;
        /* And hide the input's outline, so the form looks like the outline */
        border: var(--passwordInputBorder, none);
    }

    /* remove the input focus blue box, it will be in the wrong place. */
    input:focus {
        outline: var(--passwordInputFocusOutline, none);
    }

    /* Add the focus effect to the form so it contains the button */
    form:focus-within {
        outline: var(--passwordInputFocusWithinOutline, none);
    }

    button {
        /* Just a little styling to make it pretty */
        border: var(--passwordButtonBorder, none);
        background: var(--passwordButtonBackground, transparent);
        color: var(--passwordButtonColor, white);
        cursor: var(--passwordButtonCursor, pointer);
    }

    input[type="text"] + .eye {
        background-image: var(
            --passwordInputTextBackgroundImage,
            url("https://raw.githubusercontent.com/MariusRossouw/svelte-password-component/0954bf51d61e9abe8e70359c8cda1cb8f88db105/static/eye_closed.svg")
        );
    }
    input[type="password"] + .eye {
        background-image: var(
            --paswordInputPasswordBackgroundImage,
            url("https://raw.githubusercontent.com/MariusRossouw/svelte-password-component/0954bf51d61e9abe8e70359c8cda1cb8f88db105/static/eye_open.svg")
        );
    }
    .icon {
        padding-left: var(--passwordIconPaddingLeft, 25px);
        margin-left: var(--passwordIconMarginLeft, 5px);
        background: var(
            --passwordIconbackground,
            url("https://raw.githubusercontent.com/MariusRossouw/svelte-password-component/0954bf51d61e9abe8e70359c8cda1cb8f88db105/static/lock.svg") no-repeat left
        );
        background-size: var(--passwordIconBackgroundSize, 13px);
    }
    .eye {
        margin-right: var(--passwordEyeMarginRight, 5px);
        height: var(--passwordEyeHeight, 20px);
        width: var(--passwordEyeWidth, 20px);
        display: var(--passwordEyeDisplay, inline);
        float: var(--passwordEyeFloat, right);
        right: var(--passwordEyeRight, 15px);
        top: var(--passwordEyeTop, 42px);
        background-size: var(--passwordEyeBackgroundSize, 100%);
        background-repeat: var(--passwordEyeBackgroundRepeat, no-repeat);
        &:hover {
            cursor: var(--passwordEyeHoverCursor, pointer);
        }
    }

    .good {
        padding-left: var(--passwordGoodPaddingLeft, 30px);
        color: var(--passwordGoodColor, olivedrab);
        border-color: var(--passwordGoodBorderColor, olivedrab);
        background-image: var(
            --passwordGoodBackgroundImage,
            url("https://raw.githubusercontent.com/MariusRossouw/svelte-password-component/0954bf51d61e9abe8e70359c8cda1cb8f88db105/static/check.svg")
        );
        background-repeat: var(--passwordGoodBackgroundRepeat, no-repeat);
    }
    .bad {
        padding-left: var(--passwordBadPaddingLeft, 30px);
        color: var(--passwordBadColor, brown);
        border-color: var(--passwordBadBorderColor, brown);
        background-image: var(
            --passwordBadBackgroundImage,
            url("https://raw.githubusercontent.com/MariusRossouw/svelte-password-component/0954bf51d61e9abe8e70359c8cda1cb8f88db105/static/cross.svg")
        );
        background-repeat: var(--passwordBadBackgroundRepeat, no-repeat);
    }
</style>
