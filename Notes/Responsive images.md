- Load a big image and scale it: **not good**
- Request the correctly sized image from the server: **good**
- Load multiple images and display the correct size: **not good**
- Use JavaScript to load the correct image: **good**
- Use the HTML `<picture>` tag: **best**
	- [polyfill for older browsers](http://scottjehl.github.io/picturefill/)

- When using responsive background-images, place all of your css for setting the background-images inside of a media query.
	- This ensures the browser **only** downloads the correctly sized image
	- [Media Query Asset Downloading Reference (A little dated)](https://timkadlec.com/2012/04/media-query-asset-downloading-results/)