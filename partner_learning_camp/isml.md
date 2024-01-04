# [ISML in SFRA](https://developer.salesforce.com/docs/commerce/b2c-commerce/guide/b2c-isml.html)

- Internet Store Markup Language
- Set of propriatary ISML tags that are used in templates
- Templates render the view
- Templates
    - Are rendered by Controllers
    - Receive JSON Model from the Controller
    - This happens using ISML and HTML tags
    - Can use custom CSS and JS using the <isscript> tag

## Tags
- `<isset>` create local variables and sets the value for them
- `<isobject>` for active data collections
- `<isinclude>` for local includes
- `<isloop>` loops over a collection
- `<isif>` conditional

## Best Practices

- Several ISML tags commonly used in SiteGenesis are now banned as SFRA offers better options
    - `<iscache>` - use cache.js middleware
    - `<isset>` - use only for variables used for <isinclude> tag
    - `<isscript>` - use only to add client side JS and CSS to template
    - `<ismodule>` - ...
    - `<iscontent>` - use controller code like `resetPasswordEmail.setContent(content, 'text/html', 'UTF-8')`
    - `<iscookie>` - use `getCookie()` from `cookie.js`. To set cookie use `window.localStorage.setItem('previousSid', previousSessionID)`
    - `<isredirect>` - use code in controller like `sindow.location.href = err.responseJSON.redirectUrl`
    - `<isstatus>` - use code in controller like `res.setStatusCode(500)` or `res.setStatus(404)`

- To print data we use the legacy term `pdict` which contains the data passed on to the template
- We send full models to the template
- ISML expressions
    - `${X}`: X can be a variable or a boolean condition

## Decorators

- A decorator template is an "encapsulating" template reused by another template
- There are two decorator templates used in SFRA
    - `checkout.isml` - Doesn't contain the header menu
    - `page.isml` - Does contain the header menu
- The `<isdecorate>` tag is used to specify which decorator to use
- The `<isreplace>` tag is used to indicate where the content of the page using the decorator should go
- Ex:
    ```<div class="page" data-action="${pdict.action}" data-querystring="${pdict.queryString}" >
    <isinclude template="/components/header/pageHeader" />
    <div role="main" id="maincontent">
                <isreplace/>
    </div>
    <isinclude template="/components/footer/pageFooter" />
    </div>```

