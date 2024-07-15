- Name usually starts with "with"
- Two ways to create:
    - Component that wraps children:  
        ```function WithClass ({classes, children}) {  
        return (  
        <div className={classes}>  
        {children}  
        </div>  
        );  
        }  
          
        function SomeComponent () {  
        return (  
        <WithClass classes="red-background">  
        <p>Hello World</p>  
        </WithClass>  
        );  
        }```
        - Name should start with capital letter since it's a component
    - Function that returns a component:  
        ```function withClass (WrappedComponent, classes) {  
        return function (props) {  
        return (  
        <div className={classes}>  
        <WrappedComponent {...props} /> // make sure to pass down props  
        </div>  
        );  
        };  
        }  
          
        withClass(function SomeComponent () {  
        return (  
        <p>Hello World</p>  
        );  
        }, 'red-background');  
          
        export default withClass(SomeComponent, 'red-background'); // can also wrap default export```
        - Name should start with a lowercase letter since it's just a regular function
        - With this technique, you need to make sure that you pass down the props object to the wrapped component, otherwise it won't receive it.