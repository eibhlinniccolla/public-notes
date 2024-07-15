- Vue instance
    - Proxying of properties
        - at runtime, all the properties in `data`, `computed`, `methods`, etc. are available on the instance
            - You can access them from outside of the Vue instance
            - Vue creates getters and setters for each property so that it can react to them when they are changed
        - You can add properties to the Vue instance from outside
            - They are not reactive, because they were not created by Vue
            - They do not have getters and getters and Vue cannot react to them
    - `$data`
        - The `data` property of the Vue instance
    - `$refs`
        - an object containing all of the html elements with a `ref` attached
        - Changes made to elements manually using `$refs` may be overwritten by Vue, because they change the DOM, but not the template which Vue uses to render
    - Components
        - You can register components with the vue instance from outside by calling `Vue.component()`
            - `Vue.component('componentName', config)`
        - Register components locally by passing them to the `components` property
            - `components` is an object where the key is the selector of the component, and the value is the component object itself
    - Templates
        - You can tell Vue where to mount the app manually by passing the selector to `$mount()`
            - This is the same as passing the selector to the `el` property
- .Vue Files
    - The object that is exported in the `<script>` tag of a .vue file behaves as a Vue instance
- Components
    - `data`
        - Data needs to be a function
            - This is because if it were an object, all instances of that component would share reference to the same object, and therefore changes to the properties of that data object would affect all component instances
            - Data as a function is executed every time the component is created, so each instance has its own distinct data object with no shared references
    - `props`
        - There are multiple ways to define props
            - Can be an array of string property names
                - `props: [ 'myProp' ]`
            - Can be an object where the key is the property name and the value is the type
                - value can also be an array of types
                - `props: { myProp: String }`
                - `props: { myProp: [String, Array] }`
        - Property Validation
            - You can make a property required by setting `required: true`  
                `props: { myProp: { required: true } }`
            - You can give a property a default value by using `default`  
                `props: { myProp: { default: 'Hello!' } }`
                - if the prop is a non-primitive type, `default` must be a function which returns an object  
                    `props: {  
                    myProp: {  
                    type: Object,  
                    default: function() {  
                    return { name: 'Sophie' };  
                    }  
                    }  
                    }`
    - slots
        - Compilation and Styling
            - Databinding for projected content is done in the `parent` component
            - Styling for projected content can be done in `either` the `parent` component or the `child` component, but the child styles will override the parent's
        - Named slots
            - In the parent component:  
                `<MyComponent>  
                <p slot="someContent">Here's the content</p>  
                </MyComponent>`
            - In the child component:  
                `<slot name="someContent"></slot>`
        - Default slots
            - If you have a named slot and an unnamed slot in a child component, any content which does not have a slot attribute defined in the parent component will be rendered in the unnamed slot.
        - Slot defaults  
            `<p>  
            <slot>This will show up if there's no content to be rendered</slot  
            </p>`
    - Dynamic Components
        - `<component>`
            - Represents the place in the template where you want to dynamically render a component
        - `:is`
            - Binds to the component that you want to render
        - Default Content
            - Any content placed between the opening and closing `<component>` tags will be rendered if `:is` is not currently bound to a component  
                `<component :is="">This content will be displayed</component>`
        - Example  
            `<template>  
            <component :is="someComponent">Default text</component>  
            </template>  
              
            <script>  
            export default {  
            components: {  
            'someComponent': SomeComponent  
            }  
            }  
            </script>`
        - Lifecycle
            - Dynamic components have two extra lifecycle hooks:
                - `activated`: called when the component is loaded into `<component>`
                - `deactivated`: called when the component is removed from `<component>`
            - By default, when switching between dynamic components, a new instance is created every time, and is destroyed when it is deactivated
            - You can override this by wrapping `<component>` in `<keep-alive>`
- Event Buses / Services
    - Create a service by creating a `new Vue()` in `App.vue` or wherever the main Vue instances is  
        `export const bus = new Vue()  
        new Vue({  
        el: '#app',  
        render: h => h(App)  
        })`
    - Emit methods from a service by calling it's `$emit` method  
        `bus.$emit('eventName', data)`
    - Listen to events coming from a service by calling `$on`  
        ```bus.$on('eventName', (data) => {  
        // do something  
        );  
        ```
    - You can use methods to centralize code and avoid having to call `$emit`  
        `export const bus = new Vue({  
        methods: {  
        doAThing(x) {  
        this.$emit('eventName', x)  
        }  
        }  
        });`