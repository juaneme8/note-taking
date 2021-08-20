# Paquetes NPM
## [shortid](https://www.npmjs.com/package/shortid)
Nos permite crear ids unique no secuenciales para usar como urls.
```js
const  shortid  =  require('shortid');

console.log(shortid.generate());
// PPBqWA9
```

## [valid-url](https://www.npmjs.com/package/valid-url)
```js
 var validUrl = require('valid-url');
  
    if (validUrl.isUri(suspect)){
        console.log('Looks like an URI');
    } else {
        console.log('Not a URI');
    }
```