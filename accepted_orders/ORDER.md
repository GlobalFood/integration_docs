Getting an order
================

GlobalFood offers two integration methods for getting new orders:

- Push (Cloud system)
- Poll (Local system)


Push (Cloud system)
----------------------

This type of integration is recommended for systems with a centralized architecture. In this scenario an endpoint is notified via webhook that one or more orders have been accepted for one or more of the system's client restaurants.


**You need to provide us with:**

1. A public HTTPS endpoint that interprets our calls over the POST method
   eg: https://www.your-domain.com/integration/orderingsystem

2. Tell us which format you like (XML or JSON)



**Map orders to correct restaurants:**

You need to identify which restaurant you need to assign an order to. Every order that we push to you contains the <restaurant_key> parameter which is unique to each restaurant location. We will provide you this key manually via email once you have created that restaurant in our system. Simply tell us the email address that you used to create the account within our system. You can see in the XML & JSON example below that the example key is 8yCPCvb3dDo1k.



**Add optional security:**

By default we send a master key alongside each request in the "Authorization" (example: e6fIguVkyG5xtT3BYGMI4rfm9iVt24YJ) header of the request. This key can be used by you to verify that the sender is us. Otherwise any third party that knows your URL https://www.your-domain.com/integration/orderingsystem could try to push a "fake" order.


### Example using JSON

We will call
```
POST https://www.your-domain.com/integration/orderingsystem
```
with the payload found in [examples](./payload/json/)



### Example using XML

We will call
 ```
POST https://www.your-domain.com/integration/orderingsystem
```
with the payload found in [examples](./payload/xml/)


Poll (Local system)
----------------------

This type of integration is recommended for systems with a decentralized architecture. In this scenario the restaurant devices does not communicate with a central endpoint for notifications. The devices poll our server every minute to check for new confirmed order notifications. An example for "Polling" is when the device is behind a firewall inside the restaurant location and does not have a connection with a server in the cloud.  When using polling, the orders will be available for an unlimited amount of time until being requested by the device.


**We need to provide you:**

A secret key to allow you to poll our system. This key must be sent in the "Authorization" header of the request. This key is unique for every restaurant location. (Example: 8yCPCvb3dDo1k). We will provide you this key manually via email once you have created that restaurant in our system. Simply tell us the email address that you used to create the account with in our system.


**Polling:**

1. The endpoint for polling is: https://pos.globalfoodsoft.com/pos/order/pop
2. Method is POST
3. API version must be set to: 2
4. You can select the format (xml or json) by setting the "Accept" header to either "application/xml" or "application/json". Default is JSON.


### Example using JSON
```
curl "https://pos.globalfoodsoft.com/pos/order/pop" \
   -X POST \
   -H "Authorization: 8yCPCvb3dDo1k" \
   -H "Accept: application/json" \
   -H "Glf-Api-Version: 2"
```

See [examples](./payload/json/) for response payload


### Example using XML
```
curl "https://pos.globalfoodsoft.com/pos/order/pop" \
   -X POST \
   -H "Authorization: 8yCPCvb3dDo1k" \
   -H "Accept: application/xml"  \
   -H "Glf-Api-Version: 2"
```

See [examples](./payload/xml/) for response payload


Order fields
------------

**DISCLAIMER:**
Results may contain some fields which are not documented here. This means that they are deprecated or only used internally. They are in the process of being removed from the Accepted Orders API. The last section includes a list of relevant fields so please expect and store only those.


**NOTES:**
- all rates are decimals, meaning that 20% will be stored as 0.2 and 0.725% will be stored as 0.00725
- some fields can contain null; in JSON null is treated as a separate type, in XML it's a self-closing element; the meaning of null is "field has not been used"


### Order Collections

The fields of an order collection are:

|Field|Type|Description|
|---|---|---|
|count  |integer         | number of orders to be fetched by the system|
|orders |array of orders | list of the orders|


### Orders

The fields of an order are:

|Field|Type|Description|
|---|---|---|
|id                   |integer|      order id|
|api_version          |integer|      version of the accepted orders api|
|type                 |string|       'pickup' or 'delivery'|
|source               |string|       source of the order; can be: <br> 'website' - restaurant website on desktop browser <br> 'mobile_widget' - restaurant website on a mobile browser <br> 'facebook' - facebook page of the restaurant <br> 'facebook_website' - facebook share page on a desktop browser <br> 'facebook_mobile_widget' - facebook share page on a mobile browser|
|payment              |string|       payment method used; can be: <br> 'CASH' - cash at the register <br> 'ONLINE' - card using online payment <br> 'CARD' - card at the register <br> 'CARD_PHONE' - card details by phone|
|fulfill_at           |string|       UTC date string of when the order will be delivered or picked up|
|instructions         |string or null|  order instructions|
|restaurant_id        |integer|      restaurant id|
|restaurant_name      |string|       name of the restaurant|
|restaurant_phone     |string|       phone of the restaurant|
|restaurant_country   |string|       country of the restaurant|
|restaurant_state     |string|       state of the restaurant|
|restaurant_city      |string|       city of the restaurant|
|restaurant_zipcode   |string|       zipcode of the restaurant|
|restaurant_street    |string|       street of the restaurant|
|restaurant_latitude  |string|       latitude of the restaurant|
|restaurant_longitude |string|       longitude of the restaurant|
|restaurant_timezone  |string|       timezone of restaurant; this is set automatically when you set the exact location in the Admin panel|
|restaurant_key       |string|       authorization key of the restaurant|
|restaurant_token     |string|       token or id of the corresponding restaurant in your system; this is set by adding it to the integration in the Admin panel|
|currency             |string|       three letter currency code <br>e.g. 'USD'|
|client_id            |integer|      client id|
|client_first_name    |string|       client first name|
|client_last_name     |string|       client last name|
|client_email         |string|       client email|
|client_phone         |string|       client phone|
|client_address       |string or null|  client delivery address; it's null when order is pickup|
|client_address_parts |object or null|     client delivery address components; it's null when order is pickup; can contain the following components: <br> - street <br> - bloc <br> - floor <br> - apartment <br> - intercom <br> - more_address (in case extended address is not enabled) <br> - zipcode <br> - city <br> - full_address (legacy orders)
|pin_skipped          |integer|  pin_skipped can be: <br> 0 - inserted address was translated into latitude & longitude using using Google API. The food client fine-tuned the pin on the map manually to achieve high accuracy. <br> 1 - inserted address was translated into latitude & longitude using using Google API. The food client however skipped fine-tuning the pin on the map thus latitude & longitude values may not be accurate|
|latitude             |string or null|  latitude of the client delivery address; null when order type is pickup|
|longitude            |string or null|  longitude of the client delivery address; null when order type is pickup|
|total_price          |float|        total including taxes|
|sub_total_price      |float|        sub-total, not including tip, delivery fee and, only in 'NET' tax calculations, taxes on items|
|tax_type             |string|       how the default taxation is applied, can be either 'NET' or 'GROSS'|
|tax_value            |float|        total value of all the taxes|
|tax_name             |string|       name of default tax  <br>e.g. 'VAT', 'Sales Tax'|
|coupons              |array of strings| list of coupon codes applied on the order|
|items                |array of order items| list of order items|


### Order Items

The fields of an order item are:

|Field|Type|Description|
|---|---|---|
|id               |integer|       order item id|
|name             |string|        order item name |
|instructions     |string or null|   order item instructions|
|type             |string|        type of order item; can be: <br>- 'item' - item on the menu <br>- 'delivery_fee' - the delivery fee <br>- 'tip' - the tip <br>- 'promo_cart' - cart promotion (which applies to the entire cart, like discount on the cart total) <br> - 'promo_item' - item promotion (which applies to child items, that have parent_id equal to the id of this item)|
|type_id          |integer or null|  id of the original menu item or promotion used to create the order item; it's null for 'delivery_fee' and 'tip'|
|parent_id        |integer or null|  usually null except if the id of the parent order item has the following two conditions: item is of type 'item' and it belongs to another item of type 'promo_item'|
|total_item_price |float|         total price of the item taking into account quantity and options. In case type is 'promo_item' then it uses the child order items. In any case it does not include discounts|
|tax_type         |string|        how taxation is applied, can be either 'NET' or 'GROSS'|
|tax_value        |float|         value of the taxes; not calculated on items of type 'item' if they are children of type 'promo_item'; it's not calculated if type is 'promo_cart'|
|tax_rate         |float|         rate used to calculate taxes; can be different for any item|
|price            |float|         base price of the item, no quantity and no options; if tax_type is 'GROSS' then it also includes the tax_value|
|quantity         |integer|       quantity of the item|
|item_discount    |float|         discount applied to the item as a result of item promotions OR the total discount of an item of type 'promo_cart'|
|cart_discount    |float|         discount applied to the item as a result of cart promotions|
|cart_discount_rate  |float|      rate used to apply all of the cart promotions; if you have multiple cart promotions (type is 'promo_cart) then their rates are added up here|
|options          |array of item options| list of item options like sizes, toppings or addons|



### Item Options

The fields of an item option are:

|Field|Type|Description|
|---|---|---|
|id               |integer|       item option id|
|name             |string|        item option name|
|group_name       |string|        name of the option group <br>e.g. name of option is Ketchup, group_name is Sauces|
|type             |string|        item option type; can be either 'option' or 'size'|
|quantity         |integer|       quantity of the item option|
|price            |float|         base price of the item option, does not use quantity|
