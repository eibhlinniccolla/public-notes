use a variable using `var()`  
        `padding: var(--box-padding);`
        - add a default value by passing a second parameter  
            `padding: var(--box-padding, 10px);`  
            `padding: var(--box-padding, var(--main-padding));`
        - You can use variables to declare new ones  
            `--box-highlight-text: var(--box-text)' with highlight';`