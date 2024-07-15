- Under the hood, React compiles JSX into calls to `React.createElement` which is why you need to have `React` in scope in order to use JSX
- You can only return 1 element from the `render()` method or a functional component
    - This is because functions in JS can only have one return value
    - Solutions:
        - Return an array of elements (because an array is a single value in JS, even though it contains a list of items)
        - Use a Higher Order Component (HOC) as a pass-through
        - Use `React.Fragment`
- `children` is actually just a special prop that you can pass directly to an element instead of having it be between opening and closing tags.
    - This...  
        ```  
        var children = "Hello world!";  
        <div>{ children }</div>  
        ```
    - is equivalent to this  
        ```  
        var children = "Hello world!";  
        <div children={children} />  
        ```
- When you are spreading props, react uses an `_extends` helper under the hood uses `Object.assign` to combine the props
    - This...  
        ```  
        <div id="app-root" {...props} className="my-class" />  
        ```
    - compiles to this:  
        ```  
        var element = React.createElement(  
        "div",  
        _extends({  
        id: "app-root"  
        }, props, {  
        className: "my-class"  
        })  
        );```
- custom components need to start with an uppercase letter
    - This is how Babel knows that they are a function or a class and to treat them as a component rather than a native HTML element
    - native HTML elements are passed as a `String` to `React.createElement`
    - custom components are passed by reference so they can be executed/instantiated by `React.createElement`
    - An exception to this is if the element name has dots in it  
        ```function (props) {  
        return <props.childComponent />  
        }  
        ```
- You can only use expressions inside curly braces to interpolate
    - In the end it is being compiled to a call to `React.createElement`
    - It wouldn't make sense to pass a `for` loop as an argument to a function