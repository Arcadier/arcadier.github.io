---
layout: page
title: "PHP-SDK"
permalink: /php-sdk/
---
# Table of contents
* [User Accounts & Passwords]()
* [Addresses]()
* [Items]()
* [Cart]()

# Initialising
Download the required libraries to your directory using the following Composer command line:
```bash
composer require arcadier/arcadier-php
```

Find the `.env` file in the following directory created: "vendor\arcadier\arcadier-php\src" and replace the variables with the relevant values:
```
CLIENT_ID = ""
CLIENT_SECRET = ""
DOMAIN = ""
PROTOCOL = ""
```

Remember to load the SDK by including the following line in all your PHP scripts:
```php
require "vendor\arcadier\arcadier-php\src\api.php";
$sdk = new ApiSdk(); #this variable does not have to be $sdk, but in this documentation, it will be used throughout
```
---
## User Accounts
### Get A User's details

**GET** **```/api/v2/users/{userID}```**
```php 
$userInfo = $sdk->getUserInfo($id, $include);
echo $userInfo;
```

Arguments:
* `$id` - *(Required)* User GUID (string) 
* `$includes` - *(Optional)* Takes the following values (string):
  * `"addresses"` 

---
### Get All Buyers

**GET** **```/api/v2/admins/{adminID}/users/?role=buyer```**
```php 
$buyerList = $sdk->getAllBuyers($keywordsParam = null, $pageSize = null, $pageNumber = null);
echo $buyerList['Records'];
```

Arguments:
* `$keywords` - *(Optional)* Search all buyers having a certain keyword in their details (name/e-mail). (string)
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)

---
### Get All Merchants
**GET** **```/api/v2/admins/{adminID}/users/?role=merchant```**
```php
$merchantList = $sdk->getAllMerchants($keywordsParam = null, $pageSize = null, $pageNumber = null)
echo $merchantList['Records'];
```

Arguments:
* `$keywords` - *(Optional)* Search all merchants having a certain keyword in their details (name/e-mail). (string)
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)

---
### Get All Users

**GET** **```/api/v2/admins/{adminID}/users/?role=buyer```**
```php
$userList = $sdk->getAllUsers($keywordsParam = null, $pageSize = null, $pageNumber = null);
echo $userList['Records'];
```

Arguments:
* `$keywords` - *(Optional)* Search all users having a certain keyword in their details (name/e-mail). (string)
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)

---
### Create Buyer Account

**POST** **```api/v2/accounts/register```**
```php
$data = [
  'Email' => 'string', //email format
  'Password' => 'string', //at least 6 characters long
  'ConfirmPassword' => 'string' //repeat 'Password' field
];
$newUser = $sdk->registerUser($data);
echo $newUser;
``` 

---
### Update User information
**PUT** **`/api/v2/users/{userID}`**
```php 
$updatedUser = $sdk->updateUserInfo($id, $data);
echo $updatedUser;
```

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$data` -  *(Optional)* If omitted, user will be unaffected.


```php
$data = [
    'Email' => 'string', 
    'FirstName' => 'string',
    'LastName' => 'string',
    'DisplayName' => 'string',
    'Description' => 'string',
    'PhoneNumber' => '',
     'Media' => [
        [
            'MediaUrl' => 'string' //URL of image
        ]
    ],
    'CustomFields' => [
        [
            'Code' => 'string',
            'Name' => 'string',
            'DataFieldType' => 'string',
            'Values' => [
                'string'
            ]
        ]
    ],
    'TimeZone' => 'string',
    'Active' => true,
    'Enabled' => true,
    'Visible' => true       
];
```

---

### Upgrade User Role
**PUT `/api/v2/admins/{adminID}/users/{userID}/roles/{role}`**
```php 
$newRole = $sdk->upgradeUserRole($id, $role);
echo $newRole;
```

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$role` - *(Required)* Takes one of the following values (string)
  * "merchant"
  * "Admin"

---

### Delete User
**DELETE `/api/v2/admins/{adminID}/users/{userID}`**
```php 
$deletedUser = $sdk->deleteUser($id);
echo $deletedUser;
```

Arguments:
* `$id` - *(Required)* User GUID (string)

---

### Get Password Reset Token
**POST `/api/v2/admins/{adminID}/password`**
```php
$data = [
    'UserID': 'string', //User GUID of user to reset password for
    'Action': 'token'
];
$result = $sdk->$resetPassword($data);
echo $result['Token'];
```

---

### Update Password
**PUT `/api/v2/users/{userID}/password`** 

Arguments:
* `$userId` - *(Required)* User GUID (string)
* `$data` - *(Required)*

```php
$data = [
  'OldPassword' => 'string', //not required if resetting password
  'Password' => 'string',
  'ConfirmPassword' => 'string',
  'ResetPasswordToken' => 'string' //Obtained from Password reset API. Required if resetting password
];

$userId = "d78635hd-h7s5-k987-u4fd-333f-hd52kf6shn76";

$updatePassword = $sdk->updatePassword($data, $userId);
echo $updatePassword['Result']; 
```

---

## Addresses
### Get User Address
**GET ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `getUserAddress($id,  $addressID)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$addressID` - *(Optional)* Address GUID (string). Ommitting it will return all addresses of that user.

---

### Create User Address
**POST ``/api/v2/users/{userID}/addresses``** is mapped to `createUserAddress($id, $data)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$data` - *(Required)*

```php
$data = [
    'Name' => 'string',
    'Line1' => 'string',
    'Line2' => 'string',
    'PostCode' => 'string',
    'Latitude' => 'string',
    'Longitude' => 'string',
    'Delivery' => true,
    'Pickup' => true,
    'SpecialInstructions' => 'string',
    'State' => 'string',
    'City' => 'string',
    'Country' => 'string', //required
    'CountryCode' => 'string' //required
];
```

---

### Update User Address
**PUT ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `updateUserAddress($id, $addressID, $data)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$addressID` - *(Required)* Address GUID (string)
* `$data` - *(Required)*

```php
$data = [
    'Name' => 'string',
    'Line1' => 'string',
    'Line2' => 'string',
    'PostCode' => 'string',
    'Latitude' => 'string',
    'Longitude' => 'string',
    'Delivery' => true,
    'Pickup' => true,
    'SpecialInstructions' => 'string',
    'State' => 'string',
    'City' => 'string',
    'Country' => 'string', //required
    'CountryCode' => 'string' //required
];
```

---

### Delete User Address
**DELETE ``/api/v2/users/{userID}/addresses/{addressID}``** is mapped to `deleteUserAddress($id, $addressID)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$addressID` - *(Optional)* Address GUID (string). Ommitting it will return all addresses of that user.

---

## Items
### Get Item Information
**GET ``/api/v2/items/{itemID}``** is mapped to `getItemInfo($id)`

Arguments:
* `$id` - *(Required)* Item GUID (string)

---

### Get All Items
**GET** **```/api/v2/items```** is mapped to `getAllItems($pageSize = null, $pageNumber = null);`

Arguments:
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)

```php
$item_list = $sdk->getAllItems();
echo $item_list['Records']; //The actual array of items is in the "Records" field of the JSON response
```

---

### Search for an item

**POST** **```/api/v2/items```** is mapped to `searchItems($data);`

Search filters are passed to the function as an object:
```php
$data = [
    'keywords' => 'string', //keyword in item name, description, or custom field
    'pagesize' => 'string', //number of results
    'Categories' =>[
        'string' //Category GUID
    ],
    'sellerID' => 'string', //merchant GUID
    'customFields' => 'string', 
	'tax' => 'string',
	'minPrice' => 0,
	'maxPrice' => 0,
	'minRating' => 0,
	'maxRating' => 0,
	'startDate' => 'unix_time',
	'endDate' => 'unix_time',
	'createdStartDate' => 'unix_time',
	'createdEndDate' => 'unix_time',
	'updatedStartDate' => 'unix_time',
	'updatedEndDate' => 'unix_time'
];

$response = $sdk->getAllItemsJsonFiltering($data);
$results = $response['Records']; //The actual array of matching items is in the "Records" field of the JSON response
```

---

### Create Item
**POST ``/api/v2/merchants/{merchantID}/items``** is mapped to `createItem($data, $merchantId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$data` - Item details (Object)

```php
$data = [
    'SKU' => 'string',
    'Name' => 'string', //required
    'BuyerDescription' => 'string', //required
    'SellerDescription' => 'string', //required
    'Price' => 0, //required
    'PriceUnit' => 'string', //required
    'StockLimited' => true, //required
    'StockQuantity' => 0, //can be ommitted if StockLimited is set to false
    'IsVisibleToCustomer' => true,
    'Active' => true,
    'IsAvailable' => true,
    'CurrencyCode' => 'string', //required
    'Categories' => [
        [
            'ID' => 'string' //Category GUID. Required
        ]
    ],
    'ShippingMethods' => [ 
        [
            'ID' => 'string' //Shipping method GUID
        ]
    ],
    'PickupAddresses' => [
        [
            'ID' => 'string' //Address GUID
        ]
    ],
    'Media' => [
        [
            'MediaUrl' => 'string' //URL of image. Required
        ]
    ],
    'Tags' => [
        'string'
    ],
    'CustomFields' => [
        [
            'Code' => 'string', //Custom field code
            'Values' => [
                'string'
            ]
        ]
    ],
    'HasChildItems' => false,
    'ChildItems' => [ //this whole object can be omitted if HasChildItems is set to false
        [
            'Variants' => [
                [
                    'Name' => 'string',
                    'GroupName' => 'string2'
                ]
            ],
            'SKU' => 'string',
            'Name' => 'string',
            'BuyerDescription' => 'string',
            'SellerDescription' => 'string',
            'Price' => 0,
            'PriceUnit' => 'string',
            'StockLimited' => true,
            'StockQuantity' => 0,
            'IsVisibleToCustomer' => true,
            'Active' => true,
            'IsAvailable' => true,
            'CurrencyCode' => 'string',
            'Categories' => [
                [
                    'ID' => 'string'
                ]
            ],
            'ShippingMethods' => [
                [
                    'ID' => 'string'
                ]
            ],
            'PickupAddresses' => [
                [
                    'ID' => 'string'
                ]
            ],
            'Media' => [
                [
                    'MediaUrl' => 'string'
                ]
            ],
            'Tags' => [
                'string'
            ]
        ]
    ]
]
```

---
### Create Listing/Booking
**POST ``/api/v2/merchants/{merchantID}/items``** is mapped to `createItem($data, $merchantId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$data` - Item details (Object)

Documentation and `$data` details can be found [here](https://apiv2.arcadier.com/#b3c71583-e9e5-4afc-8061-2705113bf571).

Fields similar to `Create Item` have the same requirement.
```php
$data = [
    'SKU' => 'string',
    'Name' => 'string',
    'BuyerDescription' => 'string',
    'SellerDescription' => 'string',
    'Price' => 0,
    'PriceUnit' => 'string',
    'StockLimited' => true,
    'StockQuantity' => 0,
    'IsVisibleToCustomer' => true,
    'Active' => true,
    'IsAvailable' => true,
    'CurrencyCode' => 'string',
    'InstantBuy' => true,
    'Negotiation' => false,
    'Categories' => [
        [
            'ID' => 'string'
        ]
    ],
    'ShippingMethods' => [
        [
            'ID' => 'string'
        ]
    ],
    'PickupAddresses' => [
        [
            'ID' => 'string'
        ]
    ],
    'Media' => [
        [
            'MediaUrl' => 'string'
        ]
    ],
    'Tags' => [
        'string'
    ],
    'CustomFields' => [
        [
            'Code' => 'string',
            'Values' => [
                'string'
            ]
        ]
    ],
    'Scheduler' => [
        'TimeZoneOffset' => -12.00,
        'TimeZoneID' => 1,
        'AllDay' => false,
        'Overnight' => false,
        'StartDateTime' => 1584748800,
        'EndDateTime' => 1587427200,
        'OpeningHours' => [
        	[
        		'Day' => 1,
        		'StartTime' => '08:00:00',
        		'EndTime' => '21:00:00',
        		'IsRestDay' => false
        	],
        	[
        		'Day' => 2,
        		'StartTime' => '08:00:00',
        		'EndTime' => '21:00:00',
        		'IsRestDay' => false
        	],
        	[
        		'Day' => 3,
        		'StartTime' => '08:00:00',
        		'EndTime' => '21:00:00',
        		'IsRestDay' => false
        	],
        	[
        		'Day' => 4,
        		'StartTime' => '08:00:00',
        		'EndTime' => '21:00:00',
        		'IsRestDay' => false
        	],
        	[
        		'Day' => 5,
        		'StartTime' => '08:00:00',
        		'EndTime' => '21:00:00',
        		'IsRestDay' => true
        	],
        	[
        		'Day' => 6,
        		'StartTime' => '18:00:00',
        		'EndTime' => '23:00:00',
        		'IsRestDay' => true
        	],
        	[
        		'Day' => 7,
        		'StartTime' => '08:00:00',
        		'EndTime' => '22:00:00',
        		'IsRestDay' => true
        	]
        ],
        'Unavailables' => [
        	[
        		'StartDateTime' => 1585094400,
        		'EndDateTime' => 1585180800,
        		'Reason' => 'My Birthday',
        		'Active' => true
        	],
        	[
        		'StartDateTime' => 1587081600,
        		'EndDateTime' => 1587222000,
        		'Reason' => 'Your Birthday',
        		'Active' => true
        	]
        ]
    ],
    'HasChildItems' => true,
    'ChildItems' => [
        [
            'Variants' => [
                [
                    'Name' => 'string',
                    'GroupName' => 'string2'
                ]
            ],
            'SKU' => 'string',
            'Name' => 'string',
            'BuyerDescription' => 'string',
            'SellerDescription' => 'string',
            'Price' => 0,
            'PriceUnit' => 'string',
            'StockLimited' => true,
            'StockQuantity' => 0,
            'IsVisibleToCustomer' => true,
            'Active' => true,
            'IsAvailable' => true,
            'CurrencyCode' => 'string',
            'Categories' => [
                [
                    'ID' => 'string'
                ]
            ],
            'ShippingMethods' => [
                [
                    'ID' => 'string'
                ]
            ],
            'PickupAddresses' => [
                [
                    'ID' => 'string'
                ]
            ],
            'Media' => [
                [
                    'MediaUrl' => 'string'
                ]
            ],
            'Tags' => [
                'string'
            ]
        ]
    ]
]
```

---
### Edit Item/Listing/Booking
**PUT ``/api/v2/merchants/{merchantID}/items/{itemID}``** is mapped to `editItem($data, $merchantId, $itemId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$itemId` - *(Required)* Item GUID (string)
* `$data` - Item details (Object)

Documentation and `$data` details can be found [here](https://apiv2.arcadier.com/#8af9bf27-a3fb-4623-b8d0-f53a67697c47).

---
### Delete Item/Listing/Booking
**DELETE ``/api/v2/merchants/{merchantID}/items/{itemID}``** is mapped to `deleteItem($merchantId, $itemId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$itemId` - *(Required)* Item GUID (string)

---
### Tag Item/Listing/Booking
**POST ``/api/v2/merchants/{merchantID}/items/{itemID}/tags``** is mapped to `tagItem($data, $merchantId, $itemId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$itemId` - *(Required)* Item GUID (string)
* `$data` - Item details (Array of strings)

```php
$data = [
    'string',
    'string'
];
```

---
### Get Item Tags
**GET ``/api/v2/tags``** is mapped to `getItemTags($pageSize = null, $pageNumber = null)`

Arguments:
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)

More about pagination [here](https://apiv2.arcadier.com/#pagination)

---
### Delete Item Tags
**DELETE ``/api/v2/tags``** is mapped to `deleteTags($data)`

```php
$data = [
    'string_1',
    'string_2'
];
```

---

## Cart
### Get Buyer's Cart
**GET ``/api/v2/users/{buyerID}/carts``**

```php
$cart = $sdk->getCart($buyerId);
echo $cart['Records'];
```
Arguments:
* `$buyerId` - *(Required)* The buyer's GUID (string)

---

### Add Item to Cart
**POST ``/api/v2/users/{buyerID}/carts``**

Arguments:
* `$buyerId` - *(Required)* The buyer's GUID (string)
* `$data`:
```php
$data = [
    'ItemDetail'=> [
        'ID'=> '00000000-0000-0000-0000-000000000000' // Required. String. Item GUID. If an item has child items, this will be the child item GUID.
    ],
    'Quantity'=> 0, //integer. Required
    'CartItemType'=> 'delivery', //optional
    'ShippingMethod'=> [
        'ID'=> '00000000-0000-0000-0000-000000000000' //optional. Shipping Method GUID 
    ]
];

$cart = $sdk->addToCart($data, $buyerId);
echo $cart;
```

---

### Edit Item in Cart
**PUT ``/api/v2/users/{buyerID}/carts/{cart-item-ID}``**

Arguments:
* `$buyerId` - *(Required)* The buyer's GUID (string)'=
* `$cartItemId` - *(Required)* The Cart Item ID obtained from the response of **Add item to Cart** API
* `$data`:
```php
$data = [
    'ItemDetail'=> [
        'ID'=> '00000000-0000-0000-0000-000000000000' // Required. String. Item GUID. If an item has child items, this will be the child item GUID.
    ],
    'Quantity'=> 0, //integer. Required
    'CartItemType'=> 'delivery', //optional
    'ShippingMethod'=> [
        'ID'=> '00000000-0000-0000-0000-000000000000' //optional. Shipping Method GUID 
    ]
];

$cart = $sdk->updateCartItem($data, $buyerId, $cartItemId);
echo $cart;
```

---
### Delete Item from Cart
**DELETE ``/api/v2/users/{buyerID}/carts/{cart-item-ID}``**

Arguments:
* `$buyerId` - *(Required)* The buyer's GUID (string)'=
* `$cartItemId` - *(Required)* The Cart Item ID obtained from the response of **Add item to Cart** API

```php
$cart = $sdk->deleteCartItem($buyerId, $cartItemId);
echo $cart;
```

---

## Orders
### Get Order by Order GUID
**GET ``/api/v2/users/{merchantID}/orders/{orderID}``**

```php
$orderInfo = $sdk->getOrder($id, $userId);
echo $orderInfo;
```
Arguments:
* `$id` - *(Required)* The order GUID (string)
* `userId` - *(Required)* Can be either the Admin GUID or the merchant GUID of the the merchant who owns the order.

---

### Get All Orders of a merchant
**GET ``/api/v2/users/{merchantID}/orders/{orderID}``**

```php
$orderList = $sdk->getOrderHistory($merchantId, $pageSize, $pageNumber);
echo $orderList;
```
Arguments:
* `$merchantId` - *(Required)* The merchant GUID (string)
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)

---

### Get All Orders within an Invoice
**GET ``/api/v2/merchants/{merchantID}/transactions/{invoiceID}``**

```php
$orderList = $sdk->getOrderInfoByInvoiceId($merchantId, $invoiceId);
echo $orderList;
```
Arguments:
* `$merchantId` - *(Required)* The merchant GUID (string)
* `$invoiceId` - *(Required)* The invoice number (string)

---

## Transactions
### Get Transaction Info by Invoice number
**GET ``/api/v2/admins/{adminID}/transactions/{invoiceID}``**

```php
$transac_info = $sdk->getTransactionInfo($invoiceNo);
echo $transac_info;
```
Arguments:
* `$invoiceNo` - *(Required)* The invoice number (string)

---

### Get all Transactions of marketplace
**GET ``/api/v2/admins/{adminID}/transactions``**

```php
$transactions = $sdk->getAllTransactions($startDate = null, $endDate = null, $pageSize = null, $pageNumber = null);
echo $transactions['Records'];
```

Arguments:
* `$pageSize` - *(Optional)* The number of results in one response. (string)
* `$pageNumber` - *(Optional)* Depending on `pageSize` and the total number of results, specifying this will display different sets of results. (string)
* `$startDate` - *(Optional)* The lower limit of the time at which you want to start filtering. (integer - Unix timestamp)
* `$endDate` - *(Optional)* The upper limit of the time at which you want to end filetering. (integer - Unix timestamp)

---

### Get all Transactions of a Buyer
**GET ``/api/v2/users/{{buyerID}}/transactions``**

```php
$transactions = $sdk->getBuyerTransactions($buyerId);
echo $transactions['Records'];
```
Arguments:
* `$buyerId` - *(Required)* The buyer GUID (string)

---

## Custom Tables
### Get Custom Table Contents
**GET ``api/v2/plugins/{{packageID}}/custom-tables/{{custom-table-name}}/``**

```php
$custom_table = $sdk->getCustomTable($packageId, $tableName);
echo $custom_table['Records'];
```

Arguments:
* `$packageId` - *(Required)* The Plug-In ID (string)
* `$tableName` - *(Required)* The table name (string)

---

### Create Row 
**POST ``/api/v2/plugins/{{packageID}}/custom-tables/{{custom-table-name}}/rows``**

```php
$data = [
    `{{column-name1}}` => `string`, // data type depends on what was configured during custom table creation
    `{{column-name2}}` => 0
];

$new_row = $sdk->createRowEntry($packageId, $tableName, $data);
echo $new_row;
```
Arguments:
* `$packageId` - *(Required)* The Plug-In ID (string)
* `$tableName` - *(Required)* The table name (string)

---

### Update Row 
**PUT ``/api/v2/plugins/{{packageID}}/custom-tables/{{custom-table-name}}/rows/{{rowId}}``**

```php
$data = [
    "{{column-name1}}": "string", // data type depends on what was configured during custom table creation
    "{{column-name2}}": 0
];

$updated_row = $sdk->editRowEntry($packageId, $tableName, $rowId, $data);
echo $update_row;
```
Arguments:
* `$packageId` - *(Required)* The Plug-In ID (string)
* `$tableName` - *(Required)* The table name (string)
* `$rowId` - *(Required)* The ID of the row (string)

---

### Delete Row 
**DELETE ``/api/v2/plugins/{{packageID}}/custom-tables/{{custom-table-name}}/rows/{{rowId}}``**

```php
$deleted_row = $sdk->deleteRowEntry($packageId, $tableName, $rowId);
echo $deleted_row;
```
Arguments:
* `$packageId` - *(Required)* The Plug-In ID (string)
* `$tableName` - *(Required)* The table name (string)
* `$rowId` - *(Required)* The ID of the row (string)

---

## Event Triggers
### Get Event Triggers
**GET ``/api/v2/event-triggers/``**

```php
$event_trigger_list = $sdk->getEventTriggers();
echo $event_trigger_list;
```

---

### Create Event Trigger
**POST ``/api/v2/event-triggers/``**

```php
$data = [
    'Uri' => 'string',  //required
    'Filters' => [      //required
        'string', 
        'string'
    ],
    'Description' => 'string',
    'IsPaused' => false,
    'Headers' => [
        'Authorization' => 'string',
        'CustomHeader' => 'string',
        'Alice' => 'Bob'
    ]
];

$new_event_trigger = $sdk->addEventTrigger($data)
echo $new_event_trigger;
```

Field definitions:
* `'Uri'` - The URL of the webhook to send the payload to.
* `'Filters' => []` - Array of names of the events which act as trigger. More details [here](https://apiv2.arcadier.com/#cda751b9-e7a4-4d50-b660-72ec9cd4d4f0).
* `'Description'` - Short description of the event trigger
* `'IsPaused'` - (Boolean) Halts the triggering of this event to the specified URI. This does not delete the event trigger.
* `'Headers' => []` - Array of headers paramaters you wish to send with the payload to the webhook server.

### Edit Event Trigger
**PUT ``/api/v2/event-triggers/{{event_trigger_ID}}``**

```php
$data = [
    'Uri' => 'string',  //required
    'Filters' => [      //required
        'string', 
        'string'
    ],
    'Description' => 'string',
    'IsPaused' => false,
    'Headers' => [
        'Authorization' => 'string',
        'CustomHeader' => 'string',
        'Alice' => 'Bob'
    ]
];

$edit_event_trigger = $sdk->updateEventTrigger($eventTriggerId, $data)
echo $edit_event_trigger;
```
Arguments:
* `$eventTriggerId` - the ID of the event trigger obtained after creating it (string)
* `$data`:

Field definitions:
* `'Uri'` - The URL of the webhook to send the payload to.
* `'Filters' => []` - Array of names of the events which act as trigger. More details [here](https://apiv2.arcadier.com/#cda751b9-e7a4-4d50-b660-72ec9cd4d4f0).
* `'Description'` - Short description of the event trigger
* `'IsPaused'` - (Boolean) Halts the triggering of this event to the specified URI. This does not delete the event trigger.
* `'Headers' => []` - Array of headers paramaters you wish to send with the payload to the webhook server.







