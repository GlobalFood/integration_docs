Fetching the restaurant's menu API (Version 2)
=============================

We need to provide you a secret key to allow you to poll our system. This key must be sent in the "Authorization" header of the request. This key is unique for every restaurant location. (Example: 8yCPCvb3dDo1k). 

You may create this key from the Restaurant Admin Panel -> Others (Search for the ... icon) -> 3rd party integrations -> Enabled integrations menu.

Select the template "Fetch Menu" and generate the key.



Making the request
------------------

1. The endpoint for fetching the menu is: https://pos.globalfoodsoft.com/pos/menu
2. Method is GET
3. API version must be set to: 2
4. You can select the format (xml or json) by setting the "Accept" header to either "application/xml" or "application/json". Default is JSON.


### Example using JSON
```
curl "https://pos.globalfoodsoft.com/pos/menu" \
   -X GET \
   -H "Authorization: 8yCPCvb3dDo1k" \
   -H "Accept: application/json" \
   -H "Glf-Api-Version: 2"
```

See [menu.json](./payload/menu.json) for response payload

### Example using XML
```
curl "https://pos.globalfoodsoft.com/pos/menu" \
   -X GET \
   -H "Authorization: 8yCPCvb3dDo1k" \
   -H "Accept: application/xml"  \
   -H "Glf-Api-Version: 2"
```

See [menu.xml](./payload/menu.xml) for response payload

   

Menu fields
-----------

**DISCLAIMER:**
Results may contain some fields which are not documented here. This means that they are deprecated or only used internally. They are in the process of being removed from the Fetch Menu API. The last section includes a list of relevant fields so please expect and store only those.

### Menus

The fields of a menu are:

|Field|Type|Description|
|---|---|---|
|id              |integer|     id of the menu|
|restaurant_id   |integer|     id of the restaurant|
|currency        |string|      3 letter currency code <br>e.g. USD|
|categories      |array of menu categories|  list of the menu categories|


### Categories

The fields of a category are:

|Field|Type|Description|
|---|---|---|
|id     |integer|     id of the menu|
|name   |string|      name of the category|
|items  |array of menu items|  list of the menu items linked with the category; can be empty|
|groups |array of option groups|  list of the option groups linked with the category; can be empty|


### Items

The fields of an item are:

|Field| Type                      |Description|
|---|---------------------------|---|
|id            | integer                   |     id of the item|
|name          | string                    |      name of the item|
|description   | string                    |      description of the item |
|price         | float                     |       price of the item|
|sizes  | array of sizes            |  list of the sizes that the item comes in; can be empty|
|groups | array of option groups    |  list of the option groups linked with the item; can be empty|
|tags | array of strings or `null` |  the list of possible string values inside the list are "HOT", "VEGETARIAN", "VEGAN", "GLUTEN_FREE", "HALAL", "NUT_FREE", "DAIRY_FREE", "RAW"; the `null` value will appear if the item has never had any tags set, otherwise it's always an array
|extras | object | See description bellow|

#### Extras

The extras object contains information for [Allergens](#allergens), [Nutritional values](#nutritional-values), [Order Types](#order-types) and [Kitchen Internal Name](#kitchen-internal-name).

##### Allergens

The Allergens will have two implementations right now. 
- menu_item_allergens_ids: The first one is an array of internal IDs for the allergens that has been deprecated and will be eliminated next year.
- menu_item_allergens_values: The seconds one is an array of objects that contain the internal id and the name of the allergen.

##### Nutritional Values

- menu_item_nutritional_values: This is an array of objects that contain the internal id of the nutritional value and the name of the nutritional value.

##### Order Types

- menu_item_order_types: An array of strings with the order types in which the item is available. If this field does not exist or is null, the item is available for all order types. 

##### Kitchen Internal Name
    
- menu_item_kitchen_internal_name: A string with the internal name to be used on kitchen tickets, if available.


### Sizes


The fields of a size are:

|Field|Type|Description|
|---|---|---|
|id            |integer|     id of the size|
|name          |string|      name of the size|
|price         |float|       price of the item|
|default       |boolean|     if this size is the default size|
|groups |array of option groups|  list of the option groups linked with the size; can be empty|
|extras | object | See description bellow|

#### Extras

The extras object contains information on [Nutritional values](#nutritional-values).
Details on Nutritional values can be found in [Items / Extras / Nutritional Values](#nutritional-values)

### Option Groups

The fields of an option group are:

|Field|Type|Description|
|---|---|---|
|id               |integer|     id of the group|
|name             |string|      name of the group|
|required         |boolean|     if selecting an item in the group is required|
|allow_quantity   |boolean|     if the group allows options to have quantity bigger than 1|
|force_max        |integer|     maximum number of selected options|
|force_min        |integer|     minimum number of selected options|


### Options

The fields of an option are:

|Field|Type|Description|
|---|---|---|
|id            |integer|     id of the option|
|name          |string|      name of the option|
|price         |float|       price of the option|
|default       |boolean|     if this option is the default option inside the group|
|extras | object | See description bellow|

#### Extras

The extras object contains information on [Kitchen Internal Name](#kitchen-internal-name).
Details on Kitchen Internal Name can be found in [Items / Extras / Kitchen Internal Name](#kitchen-internal-name).
Inside options, it uses the key "menu_option_kitchen_internal_name".

NOTES: 
* The key generated using the  "Fetch Menu" template doesn't grant you access to the Accepted Orders API. More details of how to get an order once it was accepted [here](../accepted_orders/README.md). 
