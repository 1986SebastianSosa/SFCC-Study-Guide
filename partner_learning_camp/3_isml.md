# [ISML in SFRA](https://developer.salesforce.com/docs/commerce/b2c-commerce/guide/b2c-isml.html)

- Internet Store Markup Language
- Set of propriatary ISML tags that are used in templates
- Templates render the view
- Templates
    - Are rendered by Controllers
    - Receive JSON Model from the Controller
    - This happens using ISML and HTML tags
    - Can use custom CSS and JS using the <isscript> tag
- Several SFCC Classes are included in templates. These are:
    - dw.system.Request
        - Ex. `<p>${request.httpLocale}</p>`
    - dw.system.Session
        - Ex. `<p>${session.customer.profile.firstName}</p>`
    - dw.system.StringUtils
        - Ex. `${stringUtils.https('Home-Show').toString()}`
    - dw.system.URLUtils
        - Ex. `<p>${URLUtils.url}</p>`

## Tags
- `<isset>` create local variables and sets the value for them
- `<isobject>` for active data collections
- `<isinclude>` for local (`template="..."`) or remote (`url="..."`)includes
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
    ```

    <div class="page" data-action="${pdict.action}" data-querystring="${pdict.queryString}" >
        <isinclude template="/components/header/pageHeader" />
            <div role="main" id="maincontent">
                <isreplace/>
             </div>
        <isinclude template="/components/footer/pageFooter" />
    </div>

    ```

## Local Includes

- Uses the `<isinclude>` tag to use templates inside other templates
- Any page variable from the encompasing template is available in the included template

## Remote Includes

- It's used to call another controller route and include it's rendered HTML on the current page
- Ex: `<isinclude url="${URLUtils.url('Cart-MiniCart')}"/>`
- It allows to use different caching for different components of the page
- It uses the `dw.web.URLUtils`
- No parameters get passed to the called controller when using a remote include. There are methods in the URLUtils class that allows for that
    - Ex. homePage.isml is cached for 24hs. It uses a remote include for the minicart which is not cached

## Localize ISML Templates

- templates/default has templates used by the default locale
- templates/fr_FR has templates rendering in french from France. templates/fr_CA has templates rendering in french from Canada
- In BM the locales can be found in `Merchant Tools > Site Preferencefs > Locales`
- To localize streings we use Resource Bundles found in `templates/resources`
    - For "address", for ex, the default resource is `address.properties` while for france it's `address_fr_FR.properties`
    - `address_fr_FR.properties` will override `address.properties` when the selected locale is France in the Storefront
    - To render localized strings we use the `dw.util.Resource` API
        - `${Resource.msg('msg.no.saved.addresses','address', null)}`: Key, Bundle, Default
- The en_US identifier in the URL tells the site wich resource bundles to use and which localized templates


