# Salesforce Commerce API

- APIs used for storefront cases. They have a `shopper` prefix
    - `shopper-customers`: login, register customers
    - `shopper-products`: product details, search products
- All other APIs  are intended to get instance data and are not intended for storefront use. They don't have a prefix
    - `catalogs`
    - `customers`
    - `campaigns`

## Salesforce Commerce API COnfiguration
- We need:
    - Real ID: 4 character real id for the envs
    - Intance ID: specific value that points to an instance (3 digit SB id, prd, stg, dev)
    - API Site ID: RefArch, for ex.
    - Organization ID: the concatenation of `f_ecom_{realmID}_{instanceID}`
    - Short Code: new value specific to the Salesforce Commerce API
    - API Client ID: Created in Account Manager