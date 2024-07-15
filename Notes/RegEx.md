- Look ahead and behind
    - Look ahead
        - A type of assertion that says I only want to match this expression if this other expression immediately follows it
        - Examples
            - Positive look ahead  
                `var string = "Hello world";  
                string.match(/(l.)(?=o)/g);  
                // ["ll"]`
            - Negative look ahead  
                `var str = "Hello world";  
                str.match(/(l.)(?!o)/g);  
                // ["lo", "ld"]`
    - Look behind
        - Same as a look ahead except it only matches if the expression is preceded by a certain value
        - Examples
            - Positive look behind  
                `var str = "Hello world";  
                str.match(/(?<=e)(l.)/g);  
                // ["ll"]`
                - Only match "l"s followed by any character if they're preceded by an "e"
            - Negative look behind  
                `var str = "Hello world";  
                str.match(/(?<!e)(l.)/g);  
                // ["lo", "ld"]`
                - Only match "l"s followed by any character if they're NOT preceded by an "e"
- Named capture group
- Dotall mode