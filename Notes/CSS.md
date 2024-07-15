- Resources
    - Practice
        - Grid Garden
        - Flexbox Froggy
        - Flexbox Defense
        - SS Diner
    - Articles
        - https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06#.7i1ey8j4g
    - Courses
        - CSS Grids and Flexbox for Responsive Web Design
		- ~~Introduction and Setup #project-management/story~~
			- 3 Pillars of Responsive Web Design
				- Media Queries
				- Images that resize
				- Flexible grid-based layout
		- ~~Floats #project-management/story~~
			- Attribute selectors
				- `[class*="col-"]` selects all elements with a `class` attribute containing the string `col-`
				- `[href*="http:"]` selects all links that go off-site
		- ~~Flexbox #project-management/story~~
			- `flex-basis`
				- Don't use the `width` property with flex items, use `flex-basis`
		- ~~Flexbox Grid #project-management/story~~
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
				}`
		- ~~Flexbox Exercises #project-management/story~~
		- ~~Responsive Image #project-management/story~~
			- Solutions for responsive images:
				- Load a big image and scale it: **not good**
				- Request the correctly sized image from the server: **good**
				- Load multiple images and display the correct size: **not good**
				- Use JavaScript to load the correct image: **good**
				- Use the HTML `<picture>` tag: **best**
					- [polyfill for older browsers](http://scottjehl.github.io/picturefill/)
			- When using responsive background-images, place all of your css for setting the background-images inside of a media query.
				- This ensures the browser **only** downloads the correctly sized image
				- [Media Query Asset Downloading Reference (A little dated)](https://timkadlec.com/2012/04/media-query-asset-downloading-results/)
		- ~~CSS Grid #project-management/story~~
			- Use flexbox when aligning self-contained units of content in one dimension
			- Use CSS Grid when doing entire page layouts in 2 dimensions
			- [Grid fallbacks](https://rachelandrew.co.uk/css/cheatsheets/grid-fallbacks)
		- ~~Wrapping Up #project-management/story~~
        - Egghead: CSS Fundamentals
        - Egghead: CSS Selectors in Depth
        - Egghead: Convert SCSS (Sass) to CSS-in-JS
        - Egghead: Optimize User Experience for Mobile Devices and Browsers
        - Egghead: Build Complex Layouts with CSS Grid Layout
        - Egghead: Style an Application from Start to Finish
        - Egghead: Learn Advanced CSS Layout Techniques
        - Egghead: Flexbox Fundamentals
        - Frontend Masters: CSS Grids and Flexbox for Responsive Web Design
        - Frontend Masters: [CSS In-Depth, v3](https://frontendmasters.com/workshops/css-in-depth-v3/)
        - Frontend Masters: Sass Fundamentals
        - Frontend Masters: Scalable Modular Architecture for CSS (SMACSS)
        - Frontend Masters: Motion Design with CSS
        - Frontend Masters: Advanced CSS Layouts
        - Authoring/Architecting Conventions:
        - Advanced CSS
        - [Centering in CSS: The Complete Guide](https://css-tricks.com/centering-css-complete-guide/)
- CSS
- Debugging
    - [Inspecting CSS Animations with DevTools](https://css-tricks.com/inspecting-animations-in-devtools/)
- Animations
    - Docs
        - Web Animations API
            - https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API/Using_the_Web_Animations_API
    - Libraries
        - AnimeJS
            - http://animejs.com/
        - Top 10 Animation Libraries for 2017
            - https://www.sitepoint.com/our-top-9-animation-libraries/
- CSSGrid
    - [Flexbox fallbacks for common CSSGrid implementations](http://www.gridtoflex.com/)
    - [Master CSSGrid](http://mastercssgrid.com/)

- The `media` attribute of a `<link>` element tells the browser what media the stylesheet is intended for (e.g. `print`, `tty`)
    - The default is `all`
    - If your stylesheet is not intended for `all` medias, make sure to include the `media` attribute to prevent render-blocking
- Linked or imported stylesheets **must** include a `@charset "UTF-8"` declaration


[[CSS Grid]]
[[CSS Variables]]
[[Debugging CSS]]
[[Flexbox]]
[[Responsiveness]]
[[CSS Selectors]]
[[CSS Animation]]
[[CSS Methodologies]]

[[Getting Started with CSS - Jen Kramer]]