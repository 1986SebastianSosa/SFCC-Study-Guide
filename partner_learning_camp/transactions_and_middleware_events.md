# Transactions and SFRA Middleware Events

## Custom Objects

- Custom Objects (COs) are used to store data when no System Object serves the purpose for your particular data
- COs are subclasses of `Persistent Object` (can be stored in and retrieved from CC) and `Extensible Object` (custom attributes can be added)
- COs have quotas that when are reached your app will stop working
- The quotas can be found in `Administration > Operations > Quota Status`
- To create a CO by importing a CO metadata XML file in BM `Administration > Site Development > Import & Export` or create one manually in `Administration > Site Development > Custom Object Types`.
    - After importing the file, in `Import & Export` we import the metadata from that file
    - To find the newly created CO we go to `Administration > Site Development > Custom Object Types`
    - To create a CO instance: `Merchant Tools > Custom Objects > Custom Object Editor` and click `new`

## Transactions

- To create a CO with code:
    ```
    var CustomObjectMgr = require(dw/object/CustomObjectMgr);

    //The first parameter is the CO defined in BM, the second is the primary key for CO instance
    var co = CustomObjectMgr.createCustomObject('NewsletterSubscription', 'newsletterForm.email.value');

    co.custom.<atribute> = form.formField.value; // Initialize data on the CO
    ```
- To persist the data from this CO we need a transaction:
```
//Get access to the transaction class
var txn = require('dw/system/Transaction');

//Explicit Transaction: We decide when to commit or rollback
txn.begin();
try{
//Create or modify any Persistent Object using a Mgr class (CustomObjectMgr, OrderMgr, etc)
txn.commit();
}catch(e){
//error
txn.rollback();
}

//Implicit Transaction: Commits or Rolback automatically depending on the success or failure of the code
txn.wrap(function(){
    //Create or Modify Persistent Object
})
```

## Logs

- These can be found at `Administration > Site Development > Development Setup > Log Center`
- To setup logging settings we go to `Administration > Operations > Custom Log`
- We can add custom log categories and set which log level of severity (or higher) it will show
- to use the logger class: 
```
var Logger = require(dw/system/Logger)

//Log an error to a specific custom log file for the provided category
Logger.getLogger('prefix_for_log_file', 'payment_auth_category').error(...);

//Log a warning to the general custom log file for the provided category
Logger.getLogger('payment_auth_category').warn(...);
```
- Anything unusual that occurs during your code execution will get logged automatically
- Anything that you can identify ahead of time as a possible problem would be best logged to a custom log file or category

## Middleware Chain Events

- As our controller might be extended by another controller if we have a transaction on ours that we want to make sure it finishes at the end of any route, to make sure the final changes to the data is included in it, we can listen to middleware chain events.
- For this we use the `route:beforeComplete` event:
```
this.on('route:BeforeComplete', function(req, res){
    var Transaction = require('dw/system/Transaction');
    try{
        Transaction.wrap(function(){
            ... transactional code...
        });
    }catch{
        ...error handling code...
    }
});// end route:BeforeComplete
```

## Hooks

- We use hooks to configure functionality to be called at specific points in our application flow or at specific events
- They promote modularity, encapsulation and extensibility
- Two Hook Types:
    - OCAPI Hooks (Not covered in this course)
    - Custom Hooks (SFRA Hooks)
        - We can define custom extension points and call on demand via HookMgr class methods
        - Can be executed in both OCAPI or storefront requests

### Defining and registering Hooks

- Define a "hooks" attribute in package.json to hold the path to the `hooks.json` file in the base: 
```
{
    "hooks":"./cartridge/scripts/hooks.json"
}
```
- Add the name and path to the script holding the hook in `hooks.json`
- To call the hook: `dw.system.HookMgr.callHook(<extension_point_name>, <script_method_to_call>, <argument>)`
- Ex:
```
    dw.system.HookMgr.callHook("dw.order.calculate", "calculate", basket);
```