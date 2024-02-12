# Client Side JavaScript

## Override Client-Side JavaScript

- To refer to the base script we can define a path to the base cartridge in package.json:
    ```
    "paths": {
    "base": "../storefront-reference-architecture/cartridges/app_storefront_base/"
  }
    ```
- Then we set the base to a variable in the new script
- We define the new functionality
- And we update the base script
    ```
    var base = require('base/product/base');

    function funcToOverride{
        ...
    }

     base.funcToOverride = funcToOverride;
    ```