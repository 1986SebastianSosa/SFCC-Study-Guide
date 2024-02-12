# Content Assets/Slots

## Content Assets
- They can be HTML text, graphics and videos used to give on non-product related information (support pages, special sales, size charts, etc.)
- They are stored in libraries either private (belonging to only one site) or shared (between sites)

### Managing Content Assets
- They can be found on `Merchant Tools > Content > Content Assets`
- If they're set to "searchable" they can be found in a search in the storefront
- They're categorized if they belong in a folder
- We can select a template for rendering that particular content asset

### For developing Content Assets
- We have special syntax to generate links dynamically inside Content Assets:
    - Content Link Functions - They're used inside HTML content slots and content assets to generate dynamic links
    - `$staticlink$` - Creates static link
    - `$url()$` - Creates an absolute URL that retains the protocol of the outer request
    - `$httpUrl()$` - Creates an absolute URL, but with the http protocol
    - `$httpsUrl()$`- Creates an absolute URL, but with the https protocol
    - `$include()$` - Can be used to include the result of another server call
    - `$link$` - Calls the Link pipeline in the Reference Application to call pipelines. In previous versions of B2C Commerce, this was the only way to link to pipelines. This is now deprecated

## Content Slots
- They can be embedded in any part of our storefront
- They're used to show Products, Categories, Content Assets, Recommendations or Static HTML based on a schedule
- They're created when we write an `<isslot>` tag in our code

### Managing Content Slots
- They're found in `Merchant Tools > Online Marketing > Content Slots`
- We have `Global Slots`, `Category Slots` and `Folder Slots`
    - Global Slots - They can be used globally
    - Category Slots - They're shown in pre determined Catalog Categories
        - Every category has a rendering template which holds `<isslot>` category tags.
        - They may have the same rendering template but they're configured differently so they will have different content assets, schedules, etc.
    - Folder Slots - They're organized in specific folders

### Configurating Content Slots
- We need to give it an ID, set to "enabled", set a type, select the rendering template and assign a schedule to it
- We can assign certain customer groups to show our slots
- The `Rank` property is a number that sets a hirarchy between slot configurations when there's a conflict

### `<isslot/>`
- The properties `id`,  `context` and `description` are required
- Template implicit objects - They're objects that hold the data of the content slots
    - `slotcontent.content`
    - `slotcontent.slotID`
    - `slotcontent.callout`
    - `slotcontent.custom`

### Content API
- We can access the `dw.content.ContentMgr` API to get the data on our content assets and libraries
- We can use the `getContent('folderName')` method to get the content from the passed folder
