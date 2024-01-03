# ISML in SFRA

- Internet Store Markup Language
- Set of propriatary ISML tags that are used in templates
- Templates render the view
- Templates
    - Are rendered by Controllers
    - Receive JSON Model from the Controller
    - This happens using ISML and HTML tags
    - Can use custom CSS and JS using the <isscript> tag

# Tags
- <isset> create local variables and sets the value for them
- <isobject> for active data collections
- <isinclude> for local includes
- <isloop> loops over a collection
- <isif> conditional

# Best Practices

- Several ISML tags commonly used in SiteGenesis are now banned as SFRA offers better options
    - `<iscache>` - use cache.js middleware
    - <isset> - use only for variables used for <isinclude> tag
    - <isscript> - use only to add client side JS and CSS to template
    - <ismodule> - ...
    - <iscontent> - use controller code
    - <iscookie>  use `getCookie()` from `cookie.js`. To set cookie use 

