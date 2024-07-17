- `flex-basis`
    - Don't use the `width` property with flex items, use `flex-basis`
- Using Flexbox as a grid system
    - Define a grid system by using `flex-basis` to set column widths
    - Figure out what margins you want in `%` values, then do the math to find the column widths
    - If you want a 2 column layout with `4%` margin on either side:  
        `.row {  
        display: flex;  
        flex-flow: row wrap;  
        }  
          
        .column {  
        margin-left: 4%;  
        flex: 0 0 44%;  
        /* 4 + 44 + 4 + 44 + 4 = 100% */  
        }
        `

- https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Typical_Use_Cases_of_Flexbox
- https://www.smashingmagazine.com/2018/10/flexbox-use-cases/
- https://brolik.com/blog/when-to-use-flexbox/
- https://bitsofco.de/6-reasons-to-start-using-flexbox/
- https://medium.com/@ohansemmanuel/flexbox-is-awesome-but-its-not-welcome-here-a90601c292b6
- https://dzinepress.com/2013/07/why-we-should-use-flexbox-in-front-end-development-complete-guide/