- Props with `<Route>`
    - If you are using the `component` prop, the router props (history, match, etc.) are injected automatically into the routed component
    - However this method does not allow you to manually pass props to the routed component  
        ```  
        var someVal = 3;  
        // no way to get someVal into MyComponent  
        <Route path="/some/path" component={MyComponent} />  
        ```
    - If you use the `render` prop, you can manually inject props into the routed component
    - However, if you want to inject the router props into the routed component, you need to do so manually  
        ```  
        var someVal = 3;  
        // no way to get someVal into MyComponent  
        <Route  
        path="/some/path"  
        render={(routerProps) => (  
        <MyComponent someProp={someVal} {...routerProps} />  
        )}  
        />  
        ```
- Redirecting
    - with `<Redirect>`
        - Replaces the current page on the stack
            - Hitting back button will go to page BEFORE the page you were redirected from
    - with `history.push()`
        - Adds a new page to the stack
            - Hitting back button will go to page redirected from
    - with `history.replace()`
        - Replaces current page on stack, like `<Redirect>`
    - `<Redirect>` inside a router
        - You need to wrap it in a `<Switch>` component  
            ```  
            <Switch>  
            <Route path="/new" render={() => <h1>New</h1>} />  
            <Redirect from="/old" to="/new" />  
            </Switch>  
            ```
        - You can pass params along as well as avoid using `<Switch>` by creating a `<Route>` that renders a `<Redirect>`  
            ```  
            <Route  
            path="/new/:name"  
            render={({match}) => <h1>Hello {match.params.name}</h1>}  
            />  
            <Route  
            path="/old/:name"  
            render={({match}) => (  
            <Redirect to={`/new/${match.params.name}`} />  
            )}  
            />  
            ```
- Guards
    - Guards are usually created by conditionally rendering a `<Route>` element based on some state  
        ```<Switch>  
        { isAuthenticated ? (<Route path="/" component={Dashboard} />) : null }  
        <Route path="/" component={Login} />  
        </Switch>```
- Route Params
    - Params can be separated by characters other than `/`  
        ```<Route path="/user/:id-:age" />```
    - You can validate params using regular expressions
        - Example:  
            ```<Route path="/user/:id(\d{4})>```
            - This route will only match if the `:id` param is 4 digits
        - In this way, you can put two params next to each other  
            ```  
            <Route path="/user/:date(\d{2}-\d{2}-\d{4}):extension(\.[a-z]+)" />  
              
            // example path: "/user/06-09-1993.html"  
            // results in: { date: "06-09-1993", extension: ".html" }  
            ```
- Query Params
    - Snippet to parse query params from a string:  
        ```  
        var query = new URLSearchParams(this.props.location.search);  
        for (let param of query.entries()) {  
        console.log(param); // yields ['start', '5']  
        }  
        ```
        - `URLSearchParams` is a built-in object
            - returns an object, which exposes the `entries()` method.
            - `entries()` returns an `Iterator`