# [Forms](https://developer.salesforce.com/docs/commerce/b2c-commerce/guide/b2c-forms.html)

## Forms Metadata
- These are form definitions, XML files held in the `cartridge/forms` folder
- As they are localizable we have locale folders just like the resource bundles
- During execution the form becomes a session object that can be accessed with the `server.forms.getForm()` method
- Common field form elements
    - mandatory
    - max-length
    - type
- For forms there's alsoan `<include>` tag to include other forms in it
    - Ex:
    ```
    <include formid="states" name="states" missing-error="address.state.missing" parse-error="error.message.required" value-error="error.message.required"/>
    ```
- `missing-error` or `range-error`, for example, are localizable error msgs found in resource bundles
- A controller renders the isml template to which we pass:
    - An actionURL (the URL the form will submit to)
    - The form extracted with `server.form.getForm()` (we can also clear this form with the `clear()` method)
- In the template we add any JS for client side validation and submission via AJAX
- We can use `<isprint value="${pdict.yourForm.yourField.attributes}">` to print inputs, labels, etc.
- We add an empty div with class `invalid-feedback` for the error msgs
    - Ex:
        ```
        <form action="${pdict.actionUrl}" method="POST" class="newsletter-form" <isprint value="${pdict.newsletterForm.attributes}" encoding="off" />>
            <div class="form-group required">
                <label class="form-control-label">
                    <isprint value="${pdict.newsletterForm.fname.label}" encoding="htmlcontent" />
                </label>
                <input type="input" class="form-control" id="newsletter-form-fname" <isprint
                    value="${pdict.newsletterForm.fname.attributes}" encoding="off" />>
                <div class="invalid-feedback"></div>
        ```
## Two ways to submit a form:

### Without Client JS 
- The form posts to a route directly
- We create a `post` route.
- When we call `server.forms.getForm()` we're getting all the values the user entered
- We perform the required validations and do conditional rendering depending on the validity of the values

### With Client JS: through Ajax (Most common in SFRA)
- We use Ajax to send the form data to the handler route
- The handler is very similar to the one in the No-Client-JS way of doing this but it responds with JSON instead of rendering a template, because that's what the script is expecting
- Ex:
    ```
        if (newsletterForm.valid) {
    // Show the success page
    res.json({
        success: true,
        redirectUrl: URLUtils.url('Newsletter-Success').toString()
    });
} else {
    // Handle server-side validation errors here: this is just an example
    res.setStatusCode(500);
    res.json({
        error: true,
        redirectUrl: URLUtils.url('Error-Start').toString()
    });
}
    ```
- The Client JS performs a callback with the data from the server to navigates to the success or failure page depending on the information from the data, hitting a new server route 

## CSRF (Cross Site Request Forgery) Protection

- We add a hidden element in the ISML form template
    - Ex: `<input type="hidden" name="${pdict.csrf.tokenName}" value="${pdict.csrf.token}"/>`
- We require the `csrf middleware` to generate a token in the controller and add it to the Show route midleware chain
    - Ex: 
    ```
        var csrfProtection = require('*/cartridge/scripts/middleware/csrf');
        server.get('Show', server.middleware.https, csrfProtection.generateToken, function (req, res, next) {...});
    ```
- We validate the token with the middleware in the handler route
    - Ex: 
        ```
        server.post('Handler', server.middleware.https, csrfProtection.validateAjaxRequest, function (req, res, next) {...});    
        ```