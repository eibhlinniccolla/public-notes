- [[How to create a CSS variable]]

- [[How to use a CSS variable]]
    - 
    - Use `calc()` to use variables with operators `+`, `-`, `*`, `/`  
        `--indent-xl: calc(2*var(--indent-size));`
        - You have to use `calc()` if your variable is a unitless value  
            `padding: var(--spacer)px 0; /* DOESN'T WORK */`  
            `padding: calc(var(--spacer)*1px) 0; /* WORKS */`
- Behavior
    - cascade in the same way as normal CSS properties
    - are dynamic
    - Changes to custom properties are immediately applied to all instances  
        `.closure {  
        --closureVar: 30px;  
        --font-size: var(--closureVar); /* 50px because it's reassigned below */  
          
        --closureVar: 50px;  
        }`

- When you override a previously declared property...
    - with an **invalid value**, the CSS parser treats it as a syntax error and ignores it.  
        `/* paragraph will be blue */  
        body { color: red; }  
        p { color: blue; }  
        p { color: 16px; } /* syntax error, ignored */`
    - with a **variable** which _computes_ to an invalid value, the property will assume its **inherited** value.  
        `/* paragraph will be red */  
        :root { --size: 16px; }  
        body { color: red; }  
        p { color: blue; }  
        p { color: var(--size); } /* invalid, defaults to inherited value */`