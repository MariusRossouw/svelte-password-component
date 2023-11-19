<script>
    import { onMount } from "svelte";
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let placeholder = "";
    export let password = "";
    export let type = "password";
    export let validated = false;
    export let validate = {
        length: { cnt: 12 },
        uppercase: { cnt: 1 },
        lowercase: { cnt: 1 },
        numbers: { cnt: 1 },
        special: { cnt: 1 }
    };
    export let showValidations = true;
    export let showIcon = true;
    export let showHideToggle = true;

    onMount(() => {
        handleInput({target:{value: ""}})
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
        if (validate["length"] && validate["length"].cnt) {
            validate["length"].input = {}
            validate["length"].input.length = password.length || 0;
            validate["length"].input.values = password || ""
            validate["length"].input.valid = validateLength(password.length, validate["length"].cnt) || false;
            if (validate["length"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["uppercase"] && validate["uppercase"].cnt) {
            let matchUpC = password.match(/[A-Z]/g) || [];
            validate["uppercase"].input = {}
            validate["uppercase"].input.length = matchUpC.length || 0
            validate["uppercase"].input.values = matchUpC || []
            validate["uppercase"].input.valid = validateLength(matchUpC.length, validate["uppercase"].cnt) || false
            if (validate["uppercase"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["lowercase"] && validate["lowercase"].cnt) {
            let matchLo = password.match(/[a-z]/g) || [];
            validate["lowercase"].input = {}
            validate["lowercase"].input.length = matchLo.length || 0
            validate["lowercase"].input.values = matchLo || []
            validate["lowercase"].input.valid = validateLength(
                matchLo.length,
                validate["lowercase"].cnt || false
            )
            if (validate["lowercase"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["numbers"] && validate["numbers"].cnt) {
            let matchNum = password.match(/[0-9]/g) || [];
            validate["numbers"].input = {}
            validate["numbers"].input.length = matchNum.length || 0
            validate["numbers"].input.values = matchNum || []
            validate["numbers"].input.valid = validateLength(matchNum.length, validate["numbers"].cnt) || false
            if (validate["numbers"].input.valid) {
                validLength = validLength + 1;
            }
        }

        if (validate["special"] && validate["special"].cnt) {
            let matchSp =
                password.match(/[ @#$%~`!^&*()_+\-=\[\]{};':"\\|,.<>\/?]/g) ||
                [];
            validate["special"].input = {}
            validate["special"].input.length = matchSp.length || 0
            validate["special"].input.values = matchSp || []
            validate["special"].input.valid = validateLength(matchSp.length, validate["special"].cnt) || false
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
</script>

<form>
    <input
        class={showIcon ? "icon" : ""}
        {type}
        {password}
        {placeholder}
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
            Should contain atleast {validate["uppercase"].cnt}
            uppercase charanters
        </p>
    {/if}
    {#if validate["lowercase"] && validate["lowercase"].input}
        <p class={validate["lowercase"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["lowercase"].cnt}
            lowercase charanters
        </p>
    {/if}
    {#if validate["special"] && validate["special"].input}
        <p class={validate["special"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["special"].cnt}
            special charanters
        </p>
    {/if}
    {#if validate["numbers"] && validate["numbers"].input}
        <p class={validate["numbers"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["numbers"].cnt}
            numbers
        </p>
    {/if}
    {#if validate["length"] && validate["length"].input}
        <p class={validate["length"].input.valid ? "good" : "bad"}>
            Should contain atleast {validate["length"].cnt} characters in total
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
            url("./eye_closed.svg")
        );
    }
    input[type="password"] + .eye {
        background-image: var(
            --paswordInputPasswordBackgroundImage,
            url("./eye_open.svg")
        );
    }
    .icon {
        padding-left: var(--passwordIconPaddingLeft, 25px);
        margin-left: var(--passwordIconMarginLeft, 5px);
        background: var(
            --passwordIconbackground,
            url("./lock.svg") no-repeat left
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
            url("./check.svg")
        );
        background-repeat: var(--passwordGoodBackgroundRepeat, no-repeat);
    }
    .bad {
        padding-left: var(--passwordBadPaddingLeft, 30px);
        color: var(--passwordBadColor, brown);
        border-color: var(--passwordBadBorderColor, brown);
        background-image: var(
            --passwordBadBackgroundImage,
            url("./cross.svg")
        );
        background-repeat: var(--passwordBadBackgroundRepeat, no-repeat);
    }
</style>
