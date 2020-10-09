---
layout: page
title: "Node-SDK"
permalink: /node-sdk/
---

# Initialising
To download the required libraries to your `node-modules` directory, contact us to receive the command line to execute in Node.

Create a directory for your web app's scripts. In this directory, create a `config.js` files containing your marketplace's credentials:
```javascript
module.exports = {
    client:{
        apiBaseUrl: "",  //your marketplace domain
        protocol: "", //your marketplace current SSL protocol
        clientId: "", 
        clientSecret: "",
    }
}
```

Remember to load the SDK and the `config.js` in all your scripts. For example, if you create a directory for all your scripts, then import them using the following lines at the top of your files:

```javascript
const ArcadierClient = require("path-to-sdk/sdk"); //loads the SDK resources
const config = require("./config.js"); //loads the config to autenticate usage of Arcadier"s APIs

```


## Addresses
### Get User Address
**GET ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `Addresses.getUserAddresses(userId)`

```javascript
var addresses = await client. client.Addresses.getUserAddresses(userId);
console.log(addresses);
```

Arguments:
* `userId` - *(Required)* User GUID (string)

---

### Create User Address
**POST ``/api/v2/users/{userID}/addresses``** is mapped to `Addresses.createAddress(userId, body)`


```javascript
var body = {
    "Name": "string",
    "Line1": "string",
    "Line2": "string",
    "PostCode": "string",
    "Latitude": "string",
    "Longitude": "string",
    "Delivery": true,
    "Pickup": true,
    "SpecialInstructions": "string",
    "State": "string",
    "City": "string",
    "Country": "string", //required
    "CountryCode": "string" //required
};

var newAddress = await client.Addresses.createAddress(userId, body);
console.log(newAddress);
```

Arguments:
* `userId` - *(Required)* User GUID (string)
* `body` - *(Required)*

---


### Delete User Address
**DELETE ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `Addresses.deleteAddress(userId, addressId)`

```javascript
var deletedAddress = await client.Addresses.deleteAddress(userId, addressId);
console.log(deletedAddress);
```

Arguments:
* `userId` - *(Required)* User GUID (string)
* `addressId` - *(Optional)* Address GUID (string). Ommitting it will return all addresses of that user.

---

## Items
### Get Item Information
**GET ``/api/v2/items/{itemID}``** is mapped to `Items.getItemDetails(itemId)`

```javascript
var itemDetails = await client.Items.getItemDetails(itemId);
console.log(itemDetails);
```

Arguments:
* `itemId` - *(Required)* Item GUID (string)

---

### Get All Items
**GET** **```/api/v2/items```** is mapped to `Items.getAllItems(pageSize, pageNumber)`

```javascript
var item_list = await client.Items.getAllItems(pageSize, pageNumber);
console.log(item_list);
```

Arguments:
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)


---

### Search for an item

**POST** **```/api/v2/items```** is mapped to `Items.searchItems(body)`

Search filters are passed to the function as an object:
```javascript
var body = {
    "keywords" : "string", //keyword in item name, description, or custom field
    "pagesize" : "string", //number of results
    "sellerID" : "string", //merchant GUID
    "customFields" : "string", 
    "minPrice" : 0,
    "maxPrice" : 0,
    "createdStartDate" : "unix_time",
    "createdEndDate" : "unix_time",
    "updatedStartDate" : "unix_time",
    "updatedEndDate" : "unix_time"
};

var response = await client.Items.searchItems(body);
console.log(response) //The actual array of matching items is in the "Records" field of the JSON response
```

---

### Create Item
**POST ``/api/v2/merchants/{merchantID}/items``** is mapped to `Items.createItem(body)`

```javascript
var body = {
    "merchantId": "string" //required - the merchat GUID
    "SKU" : "string",
    "Name" : "string", //required
    "BuyerDescription" : "string", //required
    "SellerDescription" : "string", //required
    "Price" : 0, //required
    "PriceUnit" : "string", //required
    "StockLimited" : true, //required
    "StockQuantity" : 0, //can be ommitted if StockLimited is set to false
    "IsVisibleToCustomer" : true,
    "Active" : true,
    "IsAvailable" : true,
    "CurrencyCode" : "string", //required
    "Categories" : [
        {
            "ID" : "string" //Category GUID. Required
        }
    ],
    "ShippingMethods" : [
        {
            "ID" : "string" //Shipping method GUID
        }
    ],
    
    "Media" : [
        {
            "MediaUrl" : "string" //URL of image. Required
        }
    ],
    "Tags" : [
        "string"
    ],
    "CustomFields" : [
        {
            "Code" : "string", //Custom field code
            "Values" : [
                "string"
            ]
        }
    ],
    "HasChildItems" : false,
    "ChildItems" : [ //this whole object can be omitted if HasChildItems is set to false
        {
            "Variants" : [
                {
                    "Name" : "string",
                    "GroupName" : "string2"
                }
            ],
            "SKU" : "string",
            "Name" : "string",
            "BuyerDescription" : "string",
            "SellerDescription" : "string",
            "Price" : 0,
            "PriceUnit" : "string",
            "StockLimited" : true,
            "StockQuantity" : 0,
            "IsVisibleToCustomer" : true,
            "Active" : true,
            "IsAvailable" : true,
            "CurrencyCode" : "string",
            "Categories" : [
                {
                    "ID" : "string"
                }
            ],
            "ShippingMethods" : [
                {
                    "ID" : "string"
                }
            ],
            
            "Media" : [
                {
                    "MediaUrl" : "string"
                }
            ],
            "Tags" : [
                "string"
            ]
        }
    ]
};

var new_item = await client.Items.createItem(body);
console.log(new_item);
```

---
### Edit Item
**PUT ``/api/v2/merchants/{merchantID}/items/{itemID}``** is mapped to `Items.EditNewItem(body)`

```javascript
var body = {
    "merchantId": "string" //required - the merchat GUID
    "SKU" : "string",
    "Name" : "string", //required
    "BuyerDescription" : "string", //required
    "SellerDescription" : "string", //required
    "Price" : 0, //required
    "PriceUnit" : "string", //required
    "StockLimited" : true, //required
    "StockQuantity" : 0, //can be ommitted if StockLimited is set to false
    "IsVisibleToCustomer" : true,
    "Active" : true,
    "IsAvailable" : true,
    "CurrencyCode" : "string", //required
    "Categories" : [
        {
            "ID" : "string" //Category GUID. Required
        }
    ],
    "ShippingMethods" : [
        {
            "ID" : "string" //Shipping method GUID
        }
    ],
    
    "Media" : [
        {
            "MediaUrl" : "string" //URL of image. Required
        }
    ],
    "Tags" : [
        "string"
    ],
    "CustomFields" : [
        {
            "Code" : "string", //Custom field code
            "Values" : [
                "string"
            ]
        }
    ],
    "HasChildItems" : false,
    "ChildItems" : [ //this whole object can be omitted if HasChildItems is set to false
        {
            "Variants" : [
                {
                    "Name" : "string",
                    "GroupName" : "string2"
                }
            ],
            "SKU" : "string",
            "Name" : "string",
            "BuyerDescription" : "string",
            "SellerDescription" : "string",
            "Price" : 0,
            "PriceUnit" : "string",
            "StockLimited" : true,
            "StockQuantity" : 0,
            "IsVisibleToCustomer" : true,
            "Active" : true,
            "IsAvailable" : true,
            "CurrencyCode" : "string",
            "Categories" : [
                {
                    "ID" : "string"
                }
            ],
            "ShippingMethods" : [
                {
                    "ID" : "string"
                }
            ],
            
            "Media" : [
                {
                    "MediaUrl" : "string"
                }
            ],
            "Tags" : [
                "string"
            ]
        }
    ]
};

var edit_item = await client.Items.EditNewItem(body);
console.log(edit_item);
```

Documentation and `body` details can be found {here}(https://apiv2.arcadier.com/#8af9bf27-a3fb-4623-b8d0-f53a67697c47).

---
### Delete Item
**DELETE ``/api/v2/merchants/{merchantID}/items/{itemID}``** is mapped to `Items.deleteItem(body)`

```javascript
var body = {
    "merchantId": "string" //merchantGUID
    "itemId": "string" //itemGUID
};

var delete_item = await client.Items.deleteItem(body);
console.log(delete_item);
```

---

## Cart
### Get Buyer's Cart
**GET ``/api/v2/users/{buyerID}/carts``** is mapped to ```Carts.getCarts(body)```

```javascript
var body = {
    "userId": "string", //user GUID
    "pageSize": 0,
    "pageNumber": 0
};

var cart = await client.Carts.getCarts(body);
console.log(cart);
```

---

### Add Item to Cart
**POST ``/api/v2/users/{buyerID}/carts``** is mapped to ```Carts.addItem(body)```

```javascript
var body = {
    "userId": "string", //user GUID
    "itemId": "string", //item GUID
    "quantity": 0, //amount of the item to add to cart
    "discount": 0 //discount amount to apply to the total of the cart
};

var cart = await client.Carts.addItem(body);
console.log(cart);
```

---

### Edit Item in Cart
**PUT ``/api/v2/users/{buyerID}/carts/{cart-item-ID}``** is mapped to ```Carts.editCart(body)```

```javascript
body = {
    "itemID": "string", //item GUID
    "userID": "string", //userGUID
    "cartID": "string", //cart itemID
    "quantity": 0,
    "discount": 0,
    "cartItemType": "delivery",
    "shippingMethodId": "string"
};

var edited_cart = await client.Carts.editCart(body);
console.log(edited_cart);
```

---
### Delete Item from Cart
**DELETE ``/api/v2/users/{buyerID}/carts/{cart-item-ID}``** is mapped to ```Carts.deleteItem(body)```


```javascript
var body = {
    "userId": "string", //user GUID
    "cartId": "string" //cart item GUID
};

var delete_cart_item = await client.Carts.deleteItem(body);
console.log(delete_cart_item);
```

---

## Orders
### Get Order by Order GUID
**GET ``/api/v2/users/{merchantID}/orders/{orderID}``** is mapped to ```Orders.getOrderDetails```

```javascript
var body = {
    "userId": "string" //user GUID
    "orderId": "string" //order GUID
};

var orderInfo = await client.Orders.getOrderDetails(body);
console.log(orderInfo);
```

---

### Get All Orders of a merchant
**GET ``/api/v2/merchants/{merchantID}/transactions``** is mapped to ```Orders.getHistory(body)```

```javascript
var body = {
    "userId": "string" //user GUID
    "keyword": "string" //keyword to search within the order
    "pageSize": 0,
    "pageNumber": 0,
    "orderStatus": "string" //the order's status
    "supplier": "string" //merchant ID of the orders
    "startDate": 0,
    "endDate": 0
};

var orderList = await client.Orders.getHistory(body);
console.log(orderList);
```

---

### Get All Orders within an Invoice
**GET ``/api/v2/merchants/{merchantID}/transactions/{invoiceID}``** is mapped to ```Orders.getHistoryDetail```

```javascript
var body = {
    "merchantId": "string" //merchant GUID
    "invoiceNo": "string" //invoice number
};

var orderList = await client.Orders.getHistoryDetail(body);
console.log(orderList);
```
---

## Transactions
### Get all Transactions of marketplace
**GET ``/api/v2/admins/{adminID}/transactions``** is mapped to ```Transactions.getTransactions(pageSize, pageNumber, keywords, startDate, endDate)```

```javascript
var transactions = await client.Transactions.getTransactions(pageSize, pageNumber, keywords, startDate, endDate);
console.log(transactions);
```

Arguments:
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)
* `keywords` - *(Optional)* Search term as keyword to find within transactions.
* `startDate` - *(Optional)* The lower limit of the time at which you want to start filtering. (integer - Unix timestamp)
* `endDate` - *(Optional)* The upper limit of the time at which you want to end filetering. (integer - Unix timestamp)

---

## Custom Tables
### Get Custom Table Contents
**GET ``api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/``** is mapped to ```CustomTables.getCustomTableContents(body)```

```javascript
var body = {
    "pluginId": "string", //The Plug-In ID 
    "tableName": "string" //The table name
};

var custom_table = await client.CustomTables.getCustomTableContents(body);
console.log(custom_table);
```

---

### Create Row 
**POST ``/api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/rows``** is mapped to ```CustomTables.createCustomTableRow(body)```

```javascript
var body = {
    "pluginId": "string", //The Plug-In ID 
    "tableName": "string" //The table name
    "request": {
        "column-name-1": "string",
        "column-name-2": "string"
    }
};

var new_row = await client.CustomTables.createCustomTableRow(body);
console.log(new_row);
```

---

### Update Row 
**PUT ``/api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/rows/{rowId}``** is mapped to ```CustomTables.updateCustomTableRow(body)``` 

```javascript
var body = {
    "pluginId": "string", //The Plug-In ID 
    "tableName": "string", //The table name
    "rowID": "string",
    "request": {
        "column-name-1": "string",
        "column-name-2": "string"
    }
};

var updated_row = await client.CustomTables.updateCustomTableRow(body);
console.log(update_row);
```
---

### Delete Row 
**DELETE ``/api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/rows/{rowId}``** is mapped to ```CustomTables.deleteCustomTableRow(body)```

```javascript
var body = {
    "pluginId": "string", //The Plug-In ID 
    "tableName": "string", //The table name
    "rowID": "string"
};

var deleted_row = await client.CustomTables.deleteCustomTableRow(body);
console.log(deleted_row);
```

---

## Marketplace
### Get Marketplace Info
**GET ``/api/v2/marketplaces/``** is mapped to ```Marketplaces.getMarketplaceInfo()```

```javascript
var mp = await client.Marketplaces.getMarketplaceInfo();
console.log(mp);
```

---
