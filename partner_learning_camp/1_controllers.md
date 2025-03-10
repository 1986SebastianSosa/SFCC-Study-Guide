# [Controllers](https://developer.salesforce.com/docs/commerce/sfra/guide/b2c-sfra-features-and-comps.html#the-modules-folder-and-the-server-module)

- Controllers handle information from the user, create ViewModels, and render pages
- The ViewModels request data from B2C Commerce, convert B2C Commerce script API objects into pure JSON objects, and apply business logic.

- Controllers and the templates they render are called in the URL as controller routes, such as `Home-Show` calls the `Home`controller and executes the `Show` route
- The routes must be exported from the controller with the line `module.exports = server.exports()`

## [Cartridge Customization](https://trailhead.salesforce.com/content/learn/modules/b2c-implement-functional-solution/b2c-customize-store-with-ref-architecture?assignmentId=a5c3m000001SUIsAAO)

- Explain why customizing an application is expected.
    - To satisfy the merchant's requirements and needs
- List two SFRA customizations.
    - Automatically set the cart to default shipping method
    - Show both the sale variants and the full price variants on the product details page
- Explain the difference between the two SFRA decorator templates.
    - page.isml: Navigation information
    - checkout.isml: Doesn't
- Explain how extending or overriding a controller can impact functionality and performance.
    - Mobile-optimized UX with streamlined mobile checkout flows 
    - Touch-friendly icons.
- List two benefits of SFRA over SiteGenesis.

## Routes

- These are ExpressJS middleware chains. Each  middleware takes three arguments, `req`, `res` and `next`. 
- `req` parses query string parameters to `req.querystring`
- `res` have:
    - `res.cacheExpiration(24)`: Sets cache to 24 hours from now
    - `res.render(templateName, data)`: renders template and assigns data to `pdict` object
    - `res.json(data)`: Prints a JSON object with the data
    - `res.setViewData(data)`: Sets the output object. Useful to add multiple objects to `pdict`
    - `setViewData`: Merges all data passed into a single object
- `next` tells the server to execute the next middleware in the chain

## Module Extension

- We can extend Controllers, Models and Scripts with `server.extend(module.superModule)`
    - With this we can access the same route and module that's available in the following cartidge in the cartridge path
- For controllers:
    - `append`: extends functionality by executing AFTER the superModule route
    - `prepend`: extends functionality by executing BEFORE the superModule route
    - `replace`: overrides the superModule route