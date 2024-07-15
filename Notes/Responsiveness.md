- Devices
    - Weirdly zoomed out
```html 
<meta 
	name="viewport" 
	content="width=device-width, 
	initial-scale=1, 
	maximum-scale=1"
>
```
- Typography
    - https://www.sitepoint.com/understanding-and-using-rem-units-in-css/
    - https://snook.ca/archives/html_and_css/font-size-with-rem
    - https://css-tricks.com/rems-ems/
    - https://www.smashingmagazine.com/2014/09/balancing-line-length-font-size-responsive-web-design/
- Images
    - When exporting an image for the web
        - Double the size, low compression
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