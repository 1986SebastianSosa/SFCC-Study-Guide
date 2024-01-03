# [Models](https://salesforcecommercecloud.github.io/b2c-dev-doc/docs/current/scriptapi/html/index.html)

- Models are a representation of data in an MVC architecture
- Two types:
    - Simple: like `store`
    - Complex: like `fullProduct`
- Complex models uses `decorators` like `attributes`, `price`, `images` so that different types of products like `product`, `productBundle`, `productSet`, etc. resuses the same information, making updating this info easier

- Store Example:
    - To call for a list of stores with the required inputs the `Stores Controller` uses `storeHelpers.getStores` method
    - The `getStores` method accesses the Stores in BM with the variable `StoreMgr` via the API `dw/catalog/StoreMgr`

- To extend the functionality of a model:
    - We call the base Model with the line `base = module.superModule`
    - We then write a function with the same signature as the base
        - ex: `function CartModel(basket){...}`
    - Then we call the base model from within our function, pass it with the keyword `this`, and also, along with any parameters the base function uses
        - ex: `function CartModel(basket){base.call(this, basket);}` after this we add the functionality with which we want to extend the base Model
    - After this we extend the model with `CartModel.prototype = Object.create(base.prototype)`
    - Lastly, we export our Model with `module.exports = CartModel`

## [Localize Messages on the Storefront](https://developer.salesforce.com/docs/commerce/b2c-commerce/guide/b2c-localization.html')

- For this we use the [dw.web.Resource](https://salesforcecommercecloud.github.io/b2c-dev-doc/docs/current/scriptapi/html/index.html?target=class_dw_web_Resource.html) API
- The resource files with the translated messages for each language can be found in `cartridges/app_storefront_base/cartridge/templates/resources` folder in the base cartridge
- The Resource API has a [msgf](https://salesforcecommercecloud.github.io/b2c-dev-doc/docs/current/scriptapi/html/index.html?target=class_dw_web_Resource.html#dw_web_Resource_msgf_String_String_String_Object_DetailAnchor) method we can use to pass arguments to it