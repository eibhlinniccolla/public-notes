---
aliases: []
---
- Resources
    - Egghead: Build a Server-rendered ReactJS Application with Next.js
    - Frontend Masters: Introduction to the JAMStack

- https://nextjs.org/
- https://nextjs.org/learn/basics/create-nextjs-app
- Requires a specific folder structure to work properly (seems like gatsby)
    - `/pages` folder
- Has its own routing, doesn't use React Router
    - (similar api)
- Does code-splitting automatically for free
- Can't access webpack config
    - This means no CSS Modules
- Allows use of [styled-jsx](https://github.com/vercel/styled-jsx) for styling
    - Example  
        ```function MyComponent () {  
        return (  
        <div>  
        <p>This should be blue!</p>  
        <style jsx>{`  
        p {  
        color: blue;  
        }  
        `}</style>  
        </div>  
        );  
        }```
- Has built in custom 404 page
    - Can override this by defining a `_error.js` page in the `/pages` folder
- Has custom lifecycle hook `getInitialProps`
    - static, async method
    - allows you to do things server-side before the page is rendered and sent to the client (querying an api, database, etc.)
    - Receives a `context` object with the following (plus other stuff)
        - Pathname
        - Query params
        - request
        - response
    - Can be set on functional components by setting the property `getInitialProps` on the function object  
        ```function MyComponent () {  
        // component body  
        }  
          
        MyComponent.getInitialProps = async function (context) {  
        // do stuff  
        }```
- Needs to be deployed on a host that can run node.js