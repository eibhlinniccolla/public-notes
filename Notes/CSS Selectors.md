---
aliases:
  - Selectors
---

- Selectors
    - `:root`
        - same as `html` but with higher specificity
    - Attribute selectors
        - `[class*="col-"]` selects all elements with a `class` attribute containing the string `col-`
        - `[href*="http:"]` selects all links that go off-site

- A **declaration block** is a collection of CSS rules
- **CSS Rulesets** are a list of selectors followed by a declaration block
- If a selector has an invalid pseudo-selector or pseudo-class, the entire declaration block will be ignored
    - The exception to this rule is if the pseudo-selector has a `-webkit-` prefix
    - Example:  
        `p:lolwhoops, { /* this whole block is skipped /*  
        background-color: green;  
        }  
          
        ::nope { /* so is this one */  
        font-size: bold;  
        }  
          
        ::-webkit-imaginary-selector,  
        ::after { /* but this one is NOT skipped */  
        color: blue;  
        }`