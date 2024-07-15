- Enhancers
    - `composeEnhancers`
- Middleware
    - Writing custom middleware
        - A middleware function is a series of nested higher-order functions:  
            ```  
            function loggerMiddleware (store) {  
            return function (next) {  
            return function (action) {  
            console.log("dispatching: " + action);  
            var result = next(action);  
            console.log("next state: " + store.getState());  
            return result;  
            }  
            }  
            }  
            ```
            - store:
            - next:
            - action:
        - Middleware is then added to the store by passing it as an argument to `applyMiddleware`, which is provided by Redux  
            ```  
            var store = createStore(  
            myReducer,  
            applyMiddleware(  
            loggerMiddleware,  
            someOtherMiddleware  
            )  
            );  
            ```
            - You can add as many middlewares as you want, and they will be executed in order
- `redux-thunk`
    - Middleware that allows action creators to not return an action, but a function which when called will dispatch an action
    - Thunk action creators return a function which takes `dispatch` and `getState` as arguments, and dispatches another action as a later time.  
        ```  
        function getUserRequest () {  
        return function (dispatch, getState) {  
        someAsyncFunc(getState().userId)  
        .then(function (user) {  
        dispatch(getUserSuccess(user));  
        });  
        };  
        }  
          
        function getUserSuccess (user) {  
        return {  
        type: "GET_USER_SUCCESS",  
        payload: user  
        };  
        }  
        ```
        - `dispatch` dispatches an action
        - `getState` gets the current state, which can be useful if the action creator needs a piece of state to dispatch the new action, and you can't/don't want to pass it as part of the action payload
- `saga`
    - https://redux-saga.js.org/
    - Sagas are generators
    - Basics
        - Create Saga middleware and apply it  
            ```  
            // store.js  
            import createSagaMiddleware from "redux-saga";  
              
            var sagaMiddleware = createSagaMiddleware();  
            var store = createStore(  
            rootReducer,  
            composeEnhancers(  
            applyMiddleware(sagaMiddleware)  
            )  
            );```
        - Create saga as generator function which dispatches an action using `put`  
            ```  
            // auth/auth.sagas.js  
            import { put } from "redux-saga/effects";  
              
            export function* clearTokenSaga (action) {  
            yield localStorage.removeItem("token");  
            yield put({  
            type: "TOKEN_CLEARED"  
            });  
            }```
            - Because it's a generator, you have to `yield` everything you want to do
        - `yield` will automatically wait for promises to resolve, so you can just wrap async actions in a `try catch` block  
            ```  
            import { put } from "redux-saga/effects";  
            // logout() returns a promise  
            import { logout } from "some/http/functions";  
              
            export function* logoutSaga (action) {  
            try {  
            // yield waits for promise to resolve or reject  
            var res = yield logout(action.email);  
            yield put({ type: "LOGOUT_SUCCESS", res });  
            } catch (error) {  
            yield put({ type: "LOGOUT_FAILED", error });  
            }  
            }```
        - Listen to actions using `takeEvery`  
            ```  
            // auth/index.js  
            import { takeEvery } from "redux-saga/effects"  
            export function* watchLogout () {  
            yield takeEvery("CLEAR_TOKEN", clearTokenSaga);  
            yield takeEvery("LOGOUT_REQUEST", logoutSaga);  
            }```
            - Order doesn't matter when `yield`ing calls to `takeEvery`
            - You're essentially registering listeners for those actions
        - Run saga when you want to start listening to actions  
            ```  
            // store.js  
            sagaMiddleware.run(watchLogout);  
            ```
    - `call`
        - Helps make generators testable
    - `all`
        - A way to yield multiple values without pausing execution
    - `takeLatest`
        - Cancels any currently executing sagas for specific action and only executes the latest one