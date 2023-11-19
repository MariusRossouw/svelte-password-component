# svelte-password-component

## Installation

1. Ensure you have node installed on your OS (v19 and above - recommended)
2. Navigate to the app where you would like to use the component and run the following in your terminal
```npm install svelte-tab-component --save```

## How to use the component

1. Inside the script tag of your .svelte file ```import { Password } from 'svelte-password-component'```
2. Inside an HTML element use the imported Password component like so
```<Password {placeholder} {showValidations} {showIcon} {showHideToggle} bind:password bind:validated bind:validate  on:passwordCheck={handlePasswordEvent}/> ```

## Props, handlers and bindings
1. ```{placeholder}``` Optional, Placeholder String in the password input field
2. ```{showValidations}``` Optional, Boolean to display build in validation checkers
3. ```{showIcon}``` Optional, Boolean to display the icon on the left of the input field
4. ```{showHideToggle}``` Optional, Boolean to display the toggle icon on the right of the input field in order to display the password as text inside the input field
5. ```bind:password``` Optional, String to bind to the password
6. ```bind:validated``` Optional, String to bind to to check if all validations have passed
7. ```bind:validate``` Optional, Object specifying what to validate against, defaulting to
```
{
  length: { cnt: 12 }, // A total of 12 character in length
  uppercase: { cnt: 1 }, // At least 1 Uppercase letter
  lowercase: { cnt: 1 }, // At lease 1 Lowercase letter
  numbers: { cnt: 1 }, // At lease 1 Number
  special: { cnt: 1 } // At least 1 Special character (@#$%~`!^&*()_+\-=\[\]{};':"\\|,.<>\/?)
}
```
8. ```on:passwordCheck={handlePasswordEvent}``` Optional, Dispatch handler from the component that returns an Object containing the following
```
  {
    password: "", // The typed password value as string
    validate: {}, // The individual validations
    validated: boolean // If the password string fulfill all validations
  }
```

## Example
```+page.svelte```
``` 
    <script>
    import { Password } from "svelte-password-component";
    let password = "";
    let placeholder = "Password";
    let validated = false;
    let validate = {
        length: { cnt: 7 },
        uppercase: { cnt: 2 },
        lowercase: { cnt: 2 },
        numbers: { cnt: 1 },
        special: { cnt: 2 }
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

```


## Styling
Can be set with variables associated with every element
```
--passwordFormBorder
--passwordFormPadding
--passwordInputBorder
--passwordInputFocusOutline
--passwordInputFocusWithinOutline
--passwordButtonBorder
--passwordButtonBackground
--passwordButtonColor
--passwordButtonCursor
--passwordInputTextBackgroundImage
--paswordInputPasswordBackgroundImage
--passwordIconPaddingLeft
--passwordIconMarginLeft
--passwordIconbackground
--passwordIconBackgroundSize
--passwordEyeMarginRight
--passwordEyeHeight
--passwordEyeWidth
--passwordEyeDisplay
--passwordEyeFloat
--passwordEyeRight
--passwordEyeTop
--passwordEyeBackgroundSize
--passwordEyeBackgroundRepeat
--passwordEyeHoverCursor
--passwordGoodPaddingLeft
--passwordGoodColor
--passwordGoodBorderColor
--passwordGoodBackgroundImage
--passwordGoodBackgroundRepeat
--passwordBadPaddingLeft
--passwordBadBackgroundImage
--passwordBadBackgroundRepeat
```

## Feedback and recommendations
Please send me feedback or recommendations for improvements at mariusrossouwcr@gmail.com. I would love to here from you. [Donations](https://www.paypal.com/paypalme/MariusFRossouw) are welcome but not necessary.


