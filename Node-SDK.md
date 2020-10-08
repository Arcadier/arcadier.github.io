---
layout: page
title: "Node-SDK"
permalink: /node-sdk/
---

# Initialising
Download the required libraries to your directory using the following npm command line:
```bash
npm i arcadier-node
```
Create a directory for your web app"s scripts. In this directory, create a `config.js` files containing your marketplace"s credentials:
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
const ArcadierClient = require("../sdk"); //loads the SDK resources
const config = require("./config.js"); //loads the config to autenticate usage of Arcadier"s APIs

```


## Addresses
### Get User Address
**GET ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `Addresses.getUserAddresses(userId)`

```javascript
var addresses = await  client. client.Addresses.getUserAddresses(userId);
console.log(addresses);
```

Arguments:
* `userId` - *(Required)* User GUID (string)

---

### Create User Address
**POST ``/api/v2/users/{userID}/addresses``** is mapped to `Addresses.createAddress(userId, body)`

Arguments:
* `userId` - *(Required)* User GUID (string)
* `body` - *(Required)*

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
var newAddress = await  client. client.Addresses.createAddress(userId, body);
console.log(newAddress);
```

---

### Update User Address
**PUT ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `updateUserAddress(id, addressID, body)`

Arguments:
* `id` - *(Required)* User GUID (string)
* `addressID` - *(Required)* Address GUID (string)
* `body` - *(Required)*

```javascript
body = {
    "Name" : "string",
    "Line1" : "string",
    "Line2" : "string",
    "PostCode" : "string",
    "Latitude" : "string",
    "Longitude" : "string",
    "Delivery" : true,
    "Pickup" : true,
    "SpecialInstructions" : "string",
    "State" : "string",
    "City" : "string",
    "Country" : "string", //required
    "CountryCode" : "string" //required
};
```

---

### Delete User Address
**DELETE ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `deleteUserAddress(id, addressID)`

Arguments:
* `id` - *(Required)* User GUID (string)
* `addressID` - *(Optional)* Address GUID (string). Ommitting it will return all addresses of that user.

---

## Items
### Get Item Information
**GET ``/api/v2/items/{itemID}``** is mapped to `getItemInfo(id)`

Arguments:
* `id` - *(Required)* Item GUID (string)

---

### Get All Items
**GET** **```/api/v2/items```** is mapped to `getAllItems(pageSize = null, pageNumber = null);`

Arguments:
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)

```javascript
item_list = await  client. getAllItems();
echo item_list{"Records"}; //The actual array of items is in the "Records" field of the JSON response
```

---

### Search for an item

**POST** **```/api/v2/items```** is mapped to `searchItems(body);`

Search filters are passed to the function as an object:
```javascript
body = {
    "keywords" : "string", //keyword in item name, description, or custom field
    "pagesize" : "string", //number of results
    "Categories" :{
        "string" //Category GUID
    },
    "sellerID" : "string", //merchant GUID
    "customFields" : "string", 
	"tax" : "string",
	"minPrice" : 0,
	"maxPrice" : 0,
	"minRating" : 0,
	"maxRating" : 0,
	"startDate" : "unix_time",
	"endDate" : "unix_time",
	"createdStartDate" : "unix_time",
	"createdEndDate" : "unix_time",
	"updatedStartDate" : "unix_time",
	"updatedEndDate" : "unix_time"
};

response = await  client. getAllItemsJsonFiltering(body);
results = response{"Records"}; //The actual array of matching items is in the "Records" field of the JSON response
```

---

### Create Item
**POST ``/api/v2/merchants/{merchantID}/items``** is mapped to `createItem(body, merchantId)`

Arguments:
* `merchantId` - *(Required)* Merchant GUID (string)
* `body` - Item details (Object)

```javascript
body = {
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
    "Categories" : {
        {
            "ID" : "string" //Category GUID. Required
        }
    },
    "ShippingMethods" : { 
        {
            "ID" : "string" //Shipping method GUID
        }
    },
    "PickupAddresses" : {
        {
            "ID" : "string" //Address GUID
        }
    },
    "Media" : {
        {
            "MediaUrl" : "string" //URL of image. Required
        }
    },
    "Tags" : {
        "string"
    },
    "CustomFields" : {
        {
            "Code" : "string", //Custom field code
            "Values" : {
                "string"
            }
        }
    },
    "HasChildItems" : false,
    "ChildItems" : { //this whole object can be omitted if HasChildItems is set to false
        {
            "Variants" : {
                {
                    "Name" : "string",
                    "GroupName" : "string2"
                }
            },
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
            "Categories" : {
                {
                    "ID" : "string"
                }
            },
            "ShippingMethods" : {
                {
                    "ID" : "string"
                }
            },
            "PickupAddresses" : {
                {
                    "ID" : "string"
                }
            },
            "Media" : {
                {
                    "MediaUrl" : "string"
                }
            },
            "Tags" : {
                "string"
            }
        }
    }
}
```

---
### Create Listing/Booking
**POST ``/api/v2/merchants/{merchantID}/items``** is mapped to `createItem(body, merchantId)`

Arguments:
* `merchantId` - *(Required)* Merchant GUID (string)
* `body` - Item details (Object)

Documentation and `body` details can be found {here}(https://apiv2.arcadier.com/#b3c71583-e9e5-4afc-8061-2705113bf571).

Fields similar to `Create Item` have the same requirement.
```javascript
body = {
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
    "InstantBuy" : true,
    "Negotiation" : false,
    "Categories" : {
        {
            "ID" : "string"
        }
    },
    "ShippingMethods" : {
        {
            "ID" : "string"
        }
    },
    "PickupAddresses" : {
        {
            "ID" : "string"
        }
    },
    "Media" : {
        {
            "MediaUrl" : "string"
        }
    },
    "Tags" : {
        "string"
    },
    "CustomFields" : {
        {
            "Code" : "string",
            "Values" : {
                "string"
            }
        }
    },
    "Scheduler" : {
        "TimeZoneOffset" : -12.00,
        "TimeZoneID" : 1,
        "AllDay" : false,
        "Overnight" : false,
        "StartDateTime" : 1584748800,
        "EndDateTime" : 1587427200,
        "OpeningHours" : {
        	{
        		"Day" : 1,
        		"StartTime" : "08:00:00",
        		"EndTime" : "21:00:00",
        		"IsRestDay" : false
        	},
        	{
        		"Day" : 2,
        		"StartTime" : "08:00:00",
        		"EndTime" : "21:00:00",
        		"IsRestDay" : false
        	},
        	{
        		"Day" : 3,
        		"StartTime" : "08:00:00",
        		"EndTime" : "21:00:00",
        		"IsRestDay" : false
        	},
        	{
        		"Day" : 4,
        		"StartTime" : "08:00:00",
        		"EndTime" : "21:00:00",
        		"IsRestDay" : false
        	},
        	{
        		"Day" : 5,
        		"StartTime" : "08:00:00",
        		"EndTime" : "21:00:00",
        		"IsRestDay" : true
        	},
        	{
        		"Day" : 6,
        		"StartTime" : "18:00:00",
        		"EndTime" : "23:00:00",
        		"IsRestDay" : true
        	},
        	{
        		"Day" : 7,
        		"StartTime" : "08:00:00",
        		"EndTime" : "22:00:00",
        		"IsRestDay" : true
        	}
        },
        "Unavailables" : {
        	{
        		"StartDateTime" : 1585094400,
        		"EndDateTime" : 1585180800,
        		"Reason" : "My Birthday",
        		"Active" : true
        	},
        	{
        		"StartDateTime" : 1587081600,
        		"EndDateTime" : 1587222000,
        		"Reason" : "Your Birthday",
        		"Active" : true
        	}
        }
    },
    "HasChildItems" : true,
    "ChildItems" : {
        {
            "Variants" : {
                {
                    "Name" : "string",
                    "GroupName" : "string2"
                }
            },
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
            "Categories" : {
                {
                    "ID" : "string"
                }
            },
            "ShippingMethods" : {
                {
                    "ID" : "string"
                }
            },
            "PickupAddresses" : {
                {
                    "ID" : "string"
                }
            },
            "Media" : {
                {
                    "MediaUrl" : "string"
                }
            },
            "Tags" : {
                "string"
            }
        }
    }
}
```

---
### Edit Item/Listing/Booking
**PUT ``/api/v2/merchants/{merchantID}/items/{itemID}``** is mapped to `editItem(body, merchantId, itemId)`

Arguments:
* `merchantId` - *(Required)* Merchant GUID (string)
* `itemId` - *(Required)* Item GUID (string)
* `body` - Item details (Object)

Documentation and `body` details can be found {here}(https://apiv2.arcadier.com/#8af9bf27-a3fb-4623-b8d0-f53a67697c47).

---
### Delete Item/Listing/Booking
**DELETE ``/api/v2/merchants/{merchantID}/items/{itemID}``** is mapped to `deleteItem(merchantId, itemId)`

Arguments:
* `merchantId` - *(Required)* Merchant GUID (string)
* `itemId` - *(Required)* Item GUID (string)

---
### Tag Item/Listing/Booking
**POST ``/api/v2/merchants/{merchantID}/items/{itemID}/tags``** is mapped to `tagItem(body, merchantId, itemId)`

Arguments:
* `merchantId` - *(Required)* Merchant GUID (string)
* `itemId` - *(Required)* Item GUID (string)
* `body` - Item details (Array of strings)

```javascript
body = {
    "string",
    "string"
};
```

---
### Get Item Tags
**GET ``/api/v2/tags``** is mapped to `getItemTags(pageSize = null, pageNumber = null)`

Arguments:
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)

More about pagination {here}(https://apiv2.arcadier.com/#pagination)

---
### Delete Item Tags
**DELETE ``/api/v2/tags``** is mapped to `deleteTags(body)`

```javascript
body = {
    "string_1",
    "string_2"
};
```

---

## Cart
### Get Buyer"s Cart
**GET ``/api/v2/users/{buyerID}/carts``**

```javascript
cart = await  client. getCart(buyerId);
echo cart{"Records"};
```
Arguments:
* `buyerId` - *(Required)* The buyer"s GUID (string)

---

### Add Item to Cart
**POST ``/api/v2/users/{buyerID}/carts``**

Arguments:
* `buyerId` - *(Required)* The buyer"s GUID (string)
* `body`:
```javascript
body = {
    "ItemDetail": {
        "ID": "00000000-0000-0000-0000-000000000000" // Required. String. Item GUID. If an item has child items, this will be the child item GUID.
    },
    "Quantity": 0, //integer. Required
    "CartItemType": "delivery", //optional
    "ShippingMethod": {
        "ID": "00000000-0000-0000-0000-000000000000" //optional. Shipping Method GUID 
    }
};

cart = await  client. addToCart(body, buyerId);
echo cart;
```

---

### Edit Item in Cart
**PUT ``/api/v2/users/{buyerID}/carts/{cart-item-ID}``**

Arguments:
* `buyerId` - *(Required)* The buyer"s GUID (string)"=
* `cartItemId` - *(Required)* The Cart Item ID obtained from the response of **Add item to Cart** API
* `body`:
```javascript
body = {
    "ItemDetail": {
        "ID": "00000000-0000-0000-0000-000000000000" // Required. String. Item GUID. If an item has child items, this will be the child item GUID.
    },
    "Quantity": 0, //integer. Required
    "CartItemType": "delivery", //optional
    "ShippingMethod": {
        "ID": "00000000-0000-0000-0000-000000000000" //optional. Shipping Method GUID 
    }
};

cart = await  client. updateCartItem(body, buyerId, cartItemId);
echo cart;
```

---
### Delete Item from Cart
**DELETE ``/api/v2/users/{buyerID}/carts/{cart-item-ID}``**

Arguments:
* `buyerId` - *(Required)* The buyer"s GUID (string)"=
* `cartItemId` - *(Required)* The Cart Item ID obtained from the response of **Add item to Cart** API

```javascript
cart = await  client. deleteCartItem(buyerId, cartItemId);
echo cart;
```

---

## Orders
### Get Order by Order GUID
**GET ``/api/v2/users/{merchantID}/orders/{orderID}``**

```javascript
orderInfo = await  client. getOrder(id, userId);
echo orderInfo;
```
Arguments:
* `id` - *(Required)* The order GUID (string)
* `userId` - *(Required)* Can be either the Admin GUID or the merchant GUID of the the merchant who owns the order.

---

### Get All Orders of a merchant
**GET ``/api/v2/users/{merchantID}/orders/{orderID}``**

```javascript
orderList = await  client. getOrderHistory(merchantId, pageSize, pageNumber);
echo orderList;
```
Arguments:
* `merchantId` - *(Required)* The merchant GUID (string)
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)

---

### Get All Orders within an Invoice
**GET ``/api/v2/merchants/{merchantID}/transactions/{invoiceID}``**

```javascript
orderList = await  client. getOrderInfoByInvoiceId(merchantId, invoiceId);
echo orderList;
```
Arguments:
* `merchantId` - *(Required)* The merchant GUID (string)
* `invoiceId` - *(Required)* The invoice number (string)

---

### Update An Order
**POST ``/api/v2/merchants/{merchantID}/orders/{orderID}``**

```javascript
body = {
    "FulfilmentStatus": "string",
    "PaymentStatus": "string",
    "Balance": 0,
    "DeliveryToAddress": {
        "ID": "00000000-0000-0000-0000-000000000000"
    },
    "CartItemType": "string",
    "Freight": 0,
    "DiscountAmount": 0,
    "Surcharge": 0,
    "CustomFields": {
        {
            "Code": "string",
            "Values": {
            	"string"
            }
        }
    }
};

updated_order = await  client. editOrder(merchantId, orderId, body)
echo updated_order;
```

Arguments:
* `merchantId` - the GUID of the merchant who is the owner of the order. This can also be the Admin GUID (string)
* `orderId` - The order GUID (string)
* `body`

Field Definitions for `body` can be found {here}(https://apiv2.arcadier.com/#5b14eb44-8967-480e-82ea-166378754b2b).

### Edit Several Orders" Details
**POST ``/api/v2/admins/{adminID}/orders``**
```javascript
body = {
    { //order #1
        "ID" : "00000000-0000-0000-0000-000000000000",
        "FulfilmentStatus": "string",
        "PaymentStatus": "string",
        "Balance": 0,
        "DeliveryToAddress": {
            "ID": "00000000-0000-0000-0000-000000000000"
        },
        "CartItemType": "string",
        "Freight": 0,
        "DiscountAmount": 0,
        "Surcharge": 0,
        "CustomFields": {
            {
                "Code": "string",
                "Values": {
                    "string"
                }
            }
        }
    },
    { //order #2
        "ID" : "00000000-0000-0000-0000-000000000000",
        "FulfilmentStatus": "string",
        "PaymentStatus": "string",
        "Balance": 0,
        "DeliveryToAddress": {
            "ID": "00000000-0000-0000-0000-000000000000"
        },
        "CartItemType": "string",
        "Freight": 0,
        "DiscountAmount": 0,
        "Surcharge": 0,
        "CustomFields": {
            {
                "Code": "string",
                "Values": {
                    "string"
                }
            }
        }
    }
};

updated_orders = await  client. updateOrders(body)
echo updated_orders;
```

Field Definitions for `body` can be found {here}(https://apiv2.arcadier.com/#02990d95-cb5f-4040-9965-a88bcb342c1c).


---

## Transactions
### Get Transaction Info by Invoice number
**GET ``/api/v2/admins/{adminID}/transactions/{invoiceID}``**

```javascript
transac_info = await  client. getTransactionInfo(invoiceNo);
echo transac_info;
```
Arguments:
* `invoiceNo` - *(Required)* The invoice number (string)

---

### Get all Transactions of marketplace
**GET ``/api/v2/admins/{adminID}/transactions``**

```javascript
transactions = await  client. getAllTransactions(startDate = null, endDate = null, pageSize = null, pageNumber = null);
echo transactions{"Records"};
```

Arguments:
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)
* `startDate` - *(Optional)* The lower limit of the time at which you want to start filtering. (integer - Unix timestamp)
* `endDate` - *(Optional)* The upper limit of the time at which you want to end filetering. (integer - Unix timestamp)

---

### Get all Transactions of a Buyer
**GET ``/api/v2/users/{buyerID}/transactions``**

```javascript
transactions = await  client. getBuyerTransactions(buyerId);
echo transactions{"Records"};
```
Arguments:
* `buyerId` - *(Required)* The buyer GUID (string)

---

### Update Array of Transaction"s Details
**PUT ``/api/v2/admins/{adminID}/invoices/{invoiceID}``**

```javascript
body = {
  {
    "Payee" : {
      "ID" : "00000000-0000-0000-0000-000000000000"
    },
    "Order" : {
      "ID" : "00000000-0000-0000-0000-000000000000"
    },
    "Total" : 0,
    "Fee" : 0,
    "Status" : "string",
    "Refunded" : false,
    "RefundedRef" : "string",
    "GatewayPayKey" : "string",
    "GatewayTransactionID" : "string",
    "GatewayStatus" : "string",
    "GatewayTimeStamp" : "string",
    "GatewayRef" : "string",
    "GatewayCorrelationId" : "string",
    "GatewaySenderId" : "string",
    "GatewaySenderRef" : "string",
    "GatewayReceiverId" : "string",
    "GatewayReceiverRef" : "string",
    "Gateway" : {
      "Code" : "string"
    },
    "DateTimePaid" : 0
  }
};

updated_transaction = await  client. updateTransaction(invoiceNo, body);
echo updated_transaction;
```

Arguments:
* `invoiceNo` - the invoice number of the invoice to be updated
* `body`

Field Definitions for `body` can be found {here}(https://apiv2.arcadier.com/#68a1094c-77b6-45fb-acc2-aec053d94a28).

---

---

## Custom Tables
### Get Custom Table Contents
**GET ``api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/``**

```javascript
custom_table = await  client. getCustomTable(packageId, tableName);
echo custom_table{"Records"};
```

Arguments:
* `packageId` - *(Required)* The Plug-In ID (string)
* `tableName` - *(Required)* The table name (string)

---

### Create Row 
**POST ``/api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/rows``**

```javascript
body = {
    `{column-name1}` : `string`, // body type depends on what was configured during custom table creation
    `{column-name2}` : 0
};

new_row = await  client. createRowEntry(packageId, tableName, body);
echo new_row;
```
Arguments:
* `packageId` - *(Required)* The Plug-In ID (string)
* `tableName` - *(Required)* The table name (string)

---

### Update Row 
**PUT ``/api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/rows/{rowId}``**

```javascript
body = {
    "{column-name1}": "string", // body type depends on what was configured during custom table creation
    "{column-name2}": 0
};

updated_row = await  client. editRowEntry(packageId, tableName, rowId, body);
echo update_row;
```
Arguments:
* `packageId` - *(Required)* The Plug-In ID (string)
* `tableName` - *(Required)* The table name (string)
* `rowId` - *(Required)* The ID of the row (string)

---

### Delete Row 
**DELETE ``/api/v2/plugins/{packageID}/custom-tables/{custom-table-name}/rows/{rowId}``**

```javascript
deleted_row = await  client. deleteRowEntry(packageId, tableName, rowId);
echo deleted_row;
```
Arguments:
* `packageId` - *(Required)* The Plug-In ID (string)
* `tableName` - *(Required)* The table name (string)
* `rowId` - *(Required)* The ID of the row (string)

---

## Event Triggers
### Get Event Triggers
**GET ``/api/v2/event-triggers/``**

```javascript
event_trigger_list = await  client. getEventTriggers();
echo event_trigger_list;
```

---

### Create Event Trigger
**POST ``/api/v2/event-triggers/``**

```javascript
body = {
    "Uri" : "string",  //required
    "Filters" : {      //required
        "string", 
        "string"
    },
    "Description" : "string",
    "IsPaused" : false,
    "Headers" : {
        "Authorization" : "string",
        "CustomHeader" : "string",
        "Alice" : "Bob"
    }
};

new_event_trigger = await  client. addEventTrigger(body);
echo new_event_trigger;
```

Field definitions:
* `"Uri"` - The URL of the webhook to send the payload to.
* `"Filters" : {}` - Array of names of the events which act as trigger. More details {here}(https://apiv2.arcadier.com/#cda751b9-e7a4-4d50-b660-72ec9cd4d4f0).
* `"Description"` - Short description of the event trigger
* `"IsPaused"` - (Boolean) Halts the triggering of this event to the specified URI. This does not delete the event trigger.
* `"Headers" : {}` - Array of headers paramaters you wish to send with the payload to the webhook server.

### Edit Event Trigger
**PUT ``/api/v2/event-triggers/{event_trigger_ID}``**

```javascript
body = {
    "Uri" : "string",  //required
    "Filters" : {      //required
        "string", 
        "string"
    },
    "Description" : "string",
    "IsPaused" : false,
    "Headers" : {
        "Authorization" : "string",
        "CustomHeader" : "string",
        "Alice" : "Bob"
    }
};

edit_event_trigger = await  client. updateEventTrigger(eventTriggerId, body);
echo edit_event_trigger;
```
Arguments:
* `eventTriggerId` - the ID of the event trigger obtained after creating it (string)
* `body`:

Field definitions:
* `"Uri"` - The URL of the webhook to send the payload to.
* `"Filters" : {}` - Array of names of the events which act as trigger. More details {here}(https://apiv2.arcadier.com/#cda751b9-e7a4-4d50-b660-72ec9cd4d4f0).
* `"Description"` - Short description of the event trigger
* `"IsPaused"` - (Boolean) Halts the triggering of this event to the specified URI. This does not delete the event trigger.
* `"Headers" : {}` - Array of headers paramaters you wish to send with the payload to the webhook server.

---

### Delete Event Trigger
**DELETE ``/api/v2/event-triggers/{event_trigger_ID}``**

```javascript
delete_event_trigger = await  client. removeEventTrigger(eventTriggerId);
echo delete_event_trigger;
```
Arguments:
* `eventTriggerId` - the ID of the event trigger obtained after creating it (string)

---

## E-Mail
### Send Email
**POST ``/api/v2/admins/{adminID}/emails``**

```javascript
body= {
    "From" : "string",
    "To" : "string",
    "Body" : "Your email content. It can be in plaintext or in HTML",
    "Subject" : "string"
};

sendEmail = await  client. sendEmail(from, to, html, subject);
echo sendEmail;
```

Argurments:
* `from` - The sender"s email (string)
* `to` - The recipient"s email (string)
* `html` - Stringified HTML content of the email
* `subject` - The subject of the email (string)

---

### Send Email for Invoice number
**POST ``/api/v2/emails``**

```javascript
sendEmail = await  client. sendEmailAfterGeneratingInvoice(invoiceNo);
echo sendEmail;
``` 

Arguments:
* `invoiceno` - The invoice number (string)

---

## Static
### Get Fulfilment Statuses
**GET ``/api/v2/static/fulfilment-statuses``**
```javascript
fulfilment_statuses = await  client. getFulfilmentStatuses();
echo fulfilment_statuses{"Records"};
```

---

### Get Currencies
**GET ``/api/v2/static/currencies``**
```javascript
currencies = await  client. getCurrencies();
echo currencies{"Records"};
```

---

### Get Countries
**GET ``/api/v2/static/countries``**
```javascript
countries = await  client. getCountries();
echo countries{"Records"};
```

---

### Get Order Statuses
**GET ``/api/v2/static/order-statuses``**
```javascript
order_statuses = await  client. getOrderStatuses();
echo order_statuses{"Records"};
```

---

### Get Payment Statuses
**GET ``/api/v2/static/payment-statuses``**
```javascript
payment_statuses = await  client. getPaymentStatuses();
echo payment_statuses{"Records"};
```

---

### Get TimeZones
**GET ``/api/v2/static/timezones``**
```javascript
timezones = await  client. getTimezones();
echo timezones{"Records"};
```

---

## Categories
### Get Category List
**GET ``/api/v2/categories``**

```javascript
category_list = await  client. getCategories(pageSize = null, pageNumber = null);
echo category_list{"Records"};
```

Arguments:
* `pageSize` - *(Optional)* The number of results in one response. (integer)
* `pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (integer)

---

### Get Category Hierarchy
**GET ``/api/v2/categories/hierarchy``**

```javascript
category_hierarchy = await  client. getCategoriesWithHierarchy();
echo category_hierarchy;
```

---

### Create a Category
**POST ``/api/v2/admins/{adminID}/categories``**

```javascript
body = {
    "Name" : "string", //required
    "Description" : "string",
    "SortOrder" : 0,
    "Media" : { //required
        {
            "MediaUrl" : "string"
        }
    },
    "ParentCategoryID" : "00000000-0000-0000-0000-000000000000",
    "Level" : 0
};

new_category = await  client. createCategory(body);
echo new_category;
```

Field definitions:
* `"Name"` - Name of the category
* `"Description"` - Short Description of the category
* `"SortOrder"` - Rank in which it will appear on category lists
* `"Media" : { "MediaUrl"}` - The URL of the image to be set for the category
* `"ParentCategoryID"` - The GUID of the category which will be the parent of this new category. Omit this field if the category is going to be a parent/main category
* `"Level"` - Parent Categories have a level of `0`. subsequent levels of child categories have higher levels.

---

### Edit a Category
**PUT ``/api/v2/admins/{adminID}/categories/{categoryID}``**

```javascript
body = {
    "Name" : "string", 
    "Description" : "string",
    "SortOrder" : 0,
    "Media" : { 
        {
            "MediaUrl" : "string"
        }
    },
    "ParentCategoryID" : "00000000-0000-0000-0000-000000000000",
    "Level" : 0
};

edited_category = await  client. updateCategory(categoryId, body);
echo edited_category;
```

Arguments:
* `categoryId` - the category GUID of the category to edit
* `body`

Field definitions:
* `"Name"` - Name of the category
* `"Description"` - Short Description of the category
* `"SortOrder"` - Rank in which it will appear on category lists
* `"Media" : { "MediaUrl"}` - The URL of the image to be set for the category
* `"ParentCategoryID"` - The GUID of the category which will be the parent of this new category. Omit this field if the category is going to be a parent/main category
* `"Level"` - Parent Categories have a level of `0`. subsequent levels of child categories have higher levels.

---

### Sort Categories
**PUT ``/api/v2/admins/{adminID}/categories``**

```javascript
body = {
    "Category GUID #1", //string
    "Category GUID #2",
    "Category GUID #3"
};

sorted_list = await  client. sortCategories(body);
echo sorted_list;
```

---

### Delete a Category
**DELETE ``/api/v2/admins/{adminID}/categories/{categoryID}``**
```javascript
deleted_category = await  client. deleteCategory(categoryId);
echo deleted_category;
```

Arguments:
* `categoryId` - the category GUID of the category to edit

---

## Marketplace
### Get Marketplace Info
**GET ``/api/v2/marketplaces/``**

```javascript
mp = await  client. getMarketplaceInfo();
echo mp;
```

---

### Update Marketplace Info
**POST ``/api/v2/marketplaces/``**

```javascript
body = {
   "CustomFields" : {
        {
            "Code" : "string", //the custom field code of an existing custom field
            "Values" : {
                "string" //the value to store. body type depends on what was configured when the custom field was created.
            }
        }
    }
};

updated_mp = await  client. updateMarketplaceInfo(body);
echo updated_mp;
```

### Customize/Shorten URL
**POST ``/api/v2/rewrite-rules``**

```javascript
body = {
    "Key" : "string",
    "Value" : "string"
};

res = await  client. customiseURL(body)
echo res;
```

Field Definitions:
* `"Key"` - The custom URL slug
* `"Value"` - The default URL slug to replace

---

## Shipping/Delivery
### Get Shipping Methods
**GET ``/api/v2/merchants/{merchantID}/shipping-methods/``**

```javascript
shipping_methods = await  client. getShippingMethods(merchantId);
echo shipping_methods;
```

### Get Delivery Rates (Weight/Price)
**GET ``/api/v2/merchants/{adminID}/shipping-methods/``**

```javascript
delivery_rates = await  client. getDeliveryRates();
echo delivery_rates;

```

### Create Shipping Method/Delivery Rate
**POST ``/api/v2/merchants/{merchantID}/shipping-methods``**

```javascript
body = {
	"Courier" : "string",
	"Price" : 0, //required
	"CombinedPrice" : 0, 
	"CurrencyCode" : "string", //required
	"Description" : "string"
};
new_shipping = await  client. createShippingMethod(merchantId, body);
echo new_shipping;
```

Arguments:
* `merchantId` - The merchant"s GUID (string)
* `body`

Field Definitions:
* `"Courier"` - The shipping method name
* `"Price"` and `"CombinedPrice"` - Detailed explanations of how to use these 2 fields together is explained {here}(https://apiv2.arcadier.com/#4fc81ed1-97b5-45ce-b1e9-51fee0a9916e).
* `"CurrencyCode"` -  The currency in which the shipping method operates, eg: USD or SGD
* `"Description"` - Short description of the shipping method.

---

### Edit Shipping Method/Delivery Rate
**PUT ``/api/v2/merchants/{merchantID}/shipping-methods/{shippingmethodID}``**

```javascript
body = {
	"Courier" : "string",
	"Price" : 0, //required
	"CombinedPrice" : 0, 
	"CurrencyCode" : "string", //required
	"Description" : "string"
};
edited_shipping = await  client. updateShippingMethod(merchantId, shippingMethodId, body)
echo edited_shipping;
```

Arguments:
* `merchantId` - The merchant"s GUID (string)
* `shippingMethodId` - The shipping GUID to be edited (string)
* `body`

Field Definitions:
* `"Courier"` - The shipping method name
* `"Price"` and `"CombinedPrice"` - Detailed explanations of how to use these 2 fields together is explained {here}(https://apiv2.arcadier.com/#4fc81ed1-97b5-45ce-b1e9-51fee0a9916e).
* `"CurrencyCode"` -  The currency in which the shipping method operates, eg: USD or SGD
* `"Description"` - Short description of the shipping method.

---

### Delete Shipping Method/Delivery Rate
**DELETE ``/api/v2/merchants/{merchantID}/shipping-methods/{shippingmethodID}``**

```javascript
deleted_shipping = await  client. deleteShippingMethod(merchantId, shippingMethodId);
echo deleted_shipping;
```

Arguments:
* `merchantId` - The merchant"s GUID (string)
* `shippingMethodId` - The shipping GUID to be deleted (string)

---

## Custom Fields
### Get Custom Field Definitions
**GET ``/api/v2/admins/{adminID}/custom-field-definitions/``**

```javascript
cf = await  client. getCustomFields()
echo cf{"Records"};
```

--- 

### Get Custom Fields of a Plug-In
**GET ``/api/v2/packages/{packageID}/custom-field-definitions``**

```javascript
plugin_cf = await  client. getPluginCustomFields(packageId);
echo plugin_cf;
```

Arguments:
* `packageId` - The ID of the Plug-In (string)

---

### Create A Custom Field
**POST ``/api/v2/admins/{adminID}/custom-field-definitions``**

```javascript
body = {
  "Name" : "string",
  "IsMandatory" : true,
  "SortOrder" : 0,
  "DataInputType" : "string",
  "DataRegex" : "string",
  "MinValue" : 0,
  "MaxValue" : 0,
  "ReferenceTable" : "string",
  "DataFieldType" : "string",
  "IsSearchable" : true,
  "IsSensitive" : true,
  "Active" : true,
  "Options" : {
    {
      "Name" : "string"
    }
  }
};

new_cf = await  client. createCustomField(body);
echo new_cf;
```

Field Definitions for `body`: All the field definitions for this request can be found {here}(https://github.com/Arcadier/Coding-Tutorials/tree/master/Creating%20Custom%20Fields).

---

### Edit A Custom Field
**PUT ``/api/v2/admins/{adminID}/custom-field-definitions/{customfield-code}``**

```javascript
body = {
  "Name" : "string",
  "IsMandatory" : true,
  "SortOrder" : 0,
  "DataInputType" : "string",
  "DataRegex" : "string",
  "MinValue" : 0,
  "MaxValue" : 0,
  "ReferenceTable" : "string",
  "DataFieldType" : "string",
  "IsSearchable" : true,
  "IsSensitive" : true,
  "Active" : true,
  "Options" : {
    {
      "Name" : "string"
    }
  }
};

updated_cf = await  client. updateCustomField(code, body)
echo updated_cf;
```

Arguments:
* `code` - The custom field code obtained after creating it/getting its definition

Field Definitions for `body`: All the field definitions for this request can be found {here}(https://github.com/Arcadier/Coding-Tutorials/tree/master/Creating%20Custom%20Fields).

---

### Delete A Custom Field
**DELETE ``/api/v2/admins/{adminID}/custom-field-definitions/{custom-field-code}``**

```javascript
deleted_cf = await  client. deleteCustomField(code);
echo deleted_cf;
```

Arguments:
* `code` - The custom field code obtained after creating it/getting its definition

---

## Checkout
### Generate Invoice and Order Details from cart
**POST ``/api/v2/users/{buyerID}/invoices/carts``**

```javascript
body = {
    "Cart Item ID #1", //the cart item ID
    "Cart Item ID #2",
    "Cart Item ID #3"
};

generate_details = await  client. generateInvoice(buyerId, body);
echo generate_details;
```

Arguments:
* `buyerID` - The BUyer"s GUID (string)
* `body`

Field Definitions in `body`: the difference between an "item ID" and a "cart item ID" is explained {here}(https://apiv2.arcadier.com/#4b0bc4da-201c-472e-8deb-1a2e1099f908)

---