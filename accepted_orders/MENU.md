Fetching the restaurant's menu
=============================

We need to provide you a secret key to allow you to poll our system. This key must be sent in the "Authorization" header of the request. This key is unique for every restaurant location. (Example: 8yCPCvb3dDo1k). We will provide you this key manually via email once you have created that restaurant in our system. Simply tell us the email address that you used to create the account with in our system.

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
Results may contain some fields which are not documented here. This means that they are deprecated or only used internally. They are in the process of being removed from the Accepted Orders API. The last section includes a list of relevant fields so please expect and store only those.

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
|sort   |integer|     sorting order of the category; default is 0|
|items  |array of menu items|  list of the menu items linked with the category; can be empty|
|groups |array of option groups|  list of the option groups linked with the category; can be empty|


### Items

The fields of an item are:

|Field|Type|Description|
|---|---|---|
|id            |integer|     id of the item|
|name          |string|      name of the item|
|description   |string|      description of the item |
|price         |float|       price of the item|
|sort          |integer|     sorting order of the item; default is 0|
|sizes  |array of sizes|  list of the sizes that the item comes in; can be empty|
|groups |array of option groups|  list of the option groups linked with the item; can be empty|


### Sizes


The fields of a size are:

|Field|Type|Description|
|---|---|---|
|id            |integer|     id of the size|
|name          |string|      name of the size|
|price         |float|       price of the item|
|default       |boolean|     if this size is the default size|
|groups |array of option groups|  list of the option groups linked with the size; can be empty|


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