- Rules of hooks:
    - Can only use in functional components or custom hooks
    - Must be at the root level of the function component, not inside another function or inside a control flow statement
    - Hooks must always be named starting with "use"
- Hooks flow
    - https://raw.githubusercontent.com/donavon/hook-flow/master/hook-flow.png
    - Start rendering component
    - Parent `useState` initialized (if initial render)
    - finish rendering Parent
    - Start rendering Child
    - Child `useState` initialized (if initial render)
    - Finish rendering Child
    - (continue like this if child has child)
    - Child `useEffect` with **no** deps cleanup, and `useEffect` with **changed** dep(s) cleanup are both called (if **not** initial render)
    - Child `useEffect` calls (in order of declaration) (if not initial render, `useEffect` calls with **empty** deps are not called again)
    - Parent `useEffect` with **no** deps cleanup, and `useEffect` with **changed** dep(s) cleanup are both called (if **not** initial render)
    - Parent `useEffect` calls (in order of declaration) (if not initial render, `useEffect` calls with **empty** deps are not called again)
- `useState`
    - You can pass a function to `useState` to initialize lazily
        - Example:  
            ```  
            var [num, setNum] = useState(  
            () => window.localStorage.getItem("num") || ""  
            );  
            ```
        - This prevents the state initializer from being evaluated on every render
        - The initializer will ONLY be called on the initial render, for the entire lifetime of the component
        - This is usually not necessary unless the initializer is an expensive operation such as parsing a JSON object or reaching into local storage
    - Updates are asynchronous  
        ```  
        var [count, setCount] = useState(0);  
        function incThree () {  
        setCount(count + 1); // count is 0  
        setCount(count + 1); // count is still 0  
        setCount(count + 1); // count is still 0  
        }  
          
        // count == 1  
        ```
    - setter function can take either a value or a function
        - Function gets queued state as argument  
            ```  
            var [count, setCount] = useState(0);  
            function incThree () {  
            setCount(c => c + 1); // c is 0  
            setCount(c => c + 1); // c is 1  
            setCount(c => c + 1); // c is 2  
            }  
              
            // count == 3  
            ```
        - Always return a value from a setter function:  
            ```  
            function inc () {  
            // don't  
            setCount(c => {  
            if (c >= max) return; // count is now undefined  
            return c + 1; // count is incremented  
            }  
            // do  
            setCount (c => {  
            if (c >= max) return c; // count is unchanged  
            return c + 1;  
            }  
            }  
            ```
    - giving `useState` a primitive vs. an object
        - If it's a primitive, it will rerender on each change
        - If you pass an object, you need to pass it a NEW object if you want to trigger a re-render
            - Simply mutating an object and then calling `setMyState` with the same object will NOT trigger a re-render
            - Sometimes this is what you want, sometimes it's not
    - React "batches" state updates
        - That simply means that calling  
            ```  
            setName('Max');  
            setAge(30);  
            ```
        - in the same synchronous (!) execution cycle (e.g. in the same function) will NOT trigger two component re-render cycles.
        - Instead, the component will only re-render once and both state updates will be applied simultaneously.
- `useEffect`
    - deps array
        - if array not provided, effect runs on every render (almost never what you want)
        - if array is empty, effect runs once when component mounts (usuall not what you want)
        - if array contains an identifier, the effect will run every time the value of that identifier changes
        - ["exhaustive deps"](https://github.com/facebook/react/issues/14920)
    - You can return a cleanup function from to the effect which will run:
        - whenever the component unmounts
        - before the component re-renders
            - useful for if you have some code that needs to run between renders
- `useRef`
    - Used if you want to persist some state between renders
    - Useful if you have some state that you want to be able to mutate without triggering a re-render
    - returns a mutable ref object whose `.current` property is initialized to the passed argument (initialValue).
    - object will persist for the full lifetime of the component
    - Essentially, useRef is like a “box” that can hold a mutable value in its .current property.
    - Can be used to access a DOM element by setting it's `ref` attribute to the ref
        - Useful because the JSX element in the render function is just that, a JSX element, not a DOM node
- `useReducer`
    - Useful when you want to be able to change state without needing to trigger unnecessary re-renders
        - If you have a `useState` at the top level, every time the state changes a new render is triggered for that component and every component underneath it
        - useReducer lets you optimize performance for components that trigger deep updates because you can pass `dispatch` down instead of callbacks.
    - Thunks
        - A thunk is a function returned from another function
        - Major idea is that it's code to be executed later
        - Here's a thing, when we hear back, IT will dispatch something
        - Custom `useThunkReducer` hook
            - Example:  
                ```  
                function useThunkReducer (reducer, initialState) {  
                var [ state, dispatch ] = useReducer(reducer, initialState);  
                  
                var thunkDispatch = useCallback(function (action) {  
                if (typeof action == "Function") {  
                action(dispatch);  
                } else {  
                dispatch(action);  
                }  
                }, [dispatch]);  
                  
                return [ state, thunkDispatch ];  
                ```
            - Takes in reducer and initial state, just like `useReducer`
            - Gets `state` and `dispatch` from `useReducer`
            - Create a function that takes in the action
                - If the action is a function, it calls the function, passing in `dispatch`
                - Else it calls `dispatch` with the action
            - The function needs to be wrapped in `useCallback` because if it's not, a new function will be created every time `useThunkReducer` is called
                - This is problematic when using the returned `thunkDispatch` function in a call to `useEffect` because it will cause an infinite loop if you add `thunkDispatch` to `useEffect`'s deps list
            - Returns the state and the function wrapping `dispatch`
            
- `React.memo` & `useMemo` & `useCallback`
    - Come back to this
    - Need to think about interaction between `useState` vs `useReducer` with `useCallback` and `React.memo`
    - Wrapping a functional component in `React.memo` makes it so that the component is only re-rendered if one of its props changed (or if it's state changed?)
    - You typically use `React.memo` to memoize _components_ and `useMemo` to memoize _values_ (such as the result of an expensive computation)
        - You CAN store components in `useMemo` but `React.memo` is more common
    - `React.memo` takes a second argument which is a custom comparison function which receives the old props and the new props, and returns a boolean to tell react if it should re-render component
    - Using `React.memo` to memoize a component  
        ```  
        // parent.js  
        function Parent () {  
        var [num, setNum] = useState(0);  
        var [letter, setLetter] = useState("a");  
          
        return (  
        <>  
        <button onClick={() => setLetter("b")}>Set to B</button>  
        {letter}  
        <Child num={num} setNum={setNum}/>  
        </>  
        );  
        }  
          
        // child.js  
        // will only re-render if props change  
        var Child = React.memo(function ({ num, setNum ) {  
        return (  
        <>  
        {num}  
        <button onClick={() => setNum(num + 1)}>Inc</button>  
        </>  
        );  
        }```
    - Using `useMemo` to memoize a component  
        ```  
        // parent.js  
        function Parent () {  
        var [num, setNum] = useState(0);  
        var [letter, setLetter] = useState("a");  
          
        // num and setNum are created by useState  
        // so they won't change and can be omitted from deps  
        var child = useMemo(() => (  
        <Child num={num} setNum={setNum}/>  
        ), []);  
          
        return (  
        <>  
        <button onClick={() => setLetter("b")}>Set to B</button>  
        {letter}  
        { child }  
        </>  
        );  
        }  
          
        // child.js  
        function Child ({ num, setNum }) {  
        return (  
        <>  
        {num}  
        <button onClick={() => setNum(num + 1)}>Inc</button>  
        </>  
        );  
        }```
- `createContext()`, `useContext()`
    - Calling `React.createContext` returns two componenents:
        - `<Provider>`
        - `<Consumer>`
    - Using context makes it more difficult to use `React.memo` and `useCallback` because those functions check props but not context
        - This isn't really a big deal unless you start running into actual performance issues. Usually the tradeoff is worth it
        - You shouldn't be using the Context api for high-frequency updates, because it's not optimized to figure out what components care about state updates and which ones don't.
- Hook patterns:
    - Replacing `this.setState` with `useReducer` to merge state
        - Old code:  
            ```  
            class MyComponent () extends React.Component {  
            state = {  
            loading: false,  
            number: 2  
            };  
              
            buttonClickHandler = () => {  
            this.setState({  
            number: 3  
            });  
            }  
            ```
        - New code:  
            ```  
            function MyComponent () {  
            var [state, setState] = useReducer(  
            (state, newState) => ({...state, ...newState}),  
            { number: 2, loading: false }  
            );  
              
            function buttonClickHandler () {  
            setState({ number: 3 });  
            }  
            }```
    - Deep equality checking in `useEffect`
        - The problem:
            - When checking if the dependencies for an effect have changed, React does simple identity comparison
            - If the dependencies are objects and are generated on every render, this will always return false because objects are reference types
        - The solution:
            - Remove the deps array so the effect runs on every render
            - Store the deps in a ref so you have access to the last render's deps in the current render
            - Preface the effect with an if check to see if the deps have changed using deep equality (lodash `isEqual`, etc.)
            - Set the deps on every render so they're always updated
            - Example:  
                ```  
                function ({queryObj}) {  
                var queryRef = useRef();  
                useEffect(() => {  
                // check if current and last are deep equal  
                if (isEqual(queryRef.current, queryObj) return;  
                // else do stuff  
                })  
                useEffect(() => {  
                // update current  
                queryRef.current = queryObj;  
                })  
                }```
    - Preventing state updates on umounted component (when api does not support canceling)
        - The problem:
            - You make an api request using a client which does not support canceling
            - Before the request finishes, the component unmounts
            - The request finishes, and you try to update state, but the component is unmounted so you get an error
        - The solution:
            - Use a ref to keep track of if the component is mounted
            - ref is updated to true once component mounts
            - ref is updated to false when component unmounts
            - ref is checked before state is updated
            - Example:  
                ```  
                function MyComponent () {  
                var [someState, setSomeState] = useState(null);  
                var mountRef = useRef(false);  
                useEffect(() => {  
                mountRef.current = true;  
                return () => (mountRef.current = false)  
                }, [])  
                  
                function query (req) {  
                someClient.request(req)  
                .then(res => {  
                if (mountRef.current) {  
                setSomeState(res);  
                }  
                });  
                }  
                }```
    - Refactor render prop component into custom hook
        - before:  
            ```  
            // Query.js  
            function Query ({query, variables, normalize, children}) {  
            // do state stuff with props  
            return children(state);  
            }  
              
            // User.js  
            function User({username}) {  
            const {logout} = useContext(GitHubContext)  
            const [filter, setFilter] = useState('')  
            return (  
            <Query  
            query={userQuery}  
            variables={{username}}  
            normalize={normalizeUserData}  
            >  
            {({fetching, data, error}) =>  
            ...  
            }  
            </Query>  
            )  
            }  
            ```
        - after:  
            ```  
            // Query.js  
            function useQuery ({ query, variables, normalize }) {  
            // do state stuff with props  
            return state;  
            }  
              
            // User.js  
            function User({username}) {  
            const {logout} = useContext(GitHubContext)  
            const [filter, setFilter] = useState('')  
            const {fetching, data, error} = useQuery({  
            query: userQuery,  
            variables: {username},  
            normalize: normalizeUserData,  
            })  
            ...  
            }```
        - You can also add a simple render-prop version of the hook for easy testing  
            ```  
            var Query = ({ children, ...props }) => children(useQuery(props));  
            ```
            - This is a good way to test custom hooks, by just turning them into a render prop component and then testing them like any other component
    - Preload component using `useEffect`
        - If a component is lazily loaded, and you know with a degree of confidence that the user will need that component soon, you can preload it from the current component
        - App:  
            ```  
            var Home = React.lazy(() => import("./views/Home");  
            var Users = React.lazy(() => import("./views/Users");  
              
            return (  
            <Suspense fallback={<p>loading...</p>}>  
            <Router>  
            <Home path="/" />  
            <Users path="/users" />  
            </Router>  
            </Suspense>  
            );```
        - On the `Home` page:  
            ```  
            function Home () {  
            useEffect(function preloadUsers () {  
            import("../Users");  
            }, [])  
            // ...  
            }```
        - Automatically starts loading `users.js` bundle in the background when the `Home` component is mounted
        - Prevents a user with a slow connection from having to wait on both the component bundle, AND any requests that component needs to make
    - Memoize expensive initial value calculation in `useReducer`
        - The problem:
            - The initial value of your reducer is the result of an expensive computation
            - It will be evaluated every time the hook is run, potentially causing performance issues
            - `useReducer` does not support passing a function to calculate the initial value
            - Example:  
                `  
                function useLocalStorageReducer () {  
                // initialValue is calculated on every render  
                var initialValue = JSON.parse(  
                window.localStorage.getItem("myItem)  
                );  
                  
                var [items, dispatch] = useReducer((state, action) => {  
                // ...  
                }, initialValue);  
                }  
                `
        - The solution:
            - Defer the calculation to a function
            - Wrap the function in `useMemo`
            - pass it an empty deps array so it is only ever calculated once
            - Example:  
                `  
                function useLocalStorageReducer () {  
                // initialValue is only calculated if deps change  
                // since deps is empty, it will never change  
                var initialValue = useMemo(() => JSON.parse(  
                window.localStorage.getItem("myItem)  
                ), []);  
                  
                var [items, dispatch] = useReducer((state, action) => {  
                // ...  
                }, initialValue);  
                }  
                `