# Arcadier PHP SDK

[![GitHub release](https://img.shields.io/github/v/release/arcadier/arcadier-php)](https://img.shields.io/github/v/release/arcadier/arcadier-php)

* [Introduction]()
* [Requirements](https://github.com/Arcadier/arcadier-php#requirements)
* [How to Install](https://github.com/Arcadier/arcadier-php#installation-via-composer)
* [Getting Started](https://github.com/Arcadier/arcadier-php#-getting-started)
  * [Authentication](https://github.com/Arcadier/arcadier-php#authentication)
  * [Trying it](https://github.com/Arcadier/arcadier-php#trying-it)
* [API Documentation](https://github.com/Arcadier/arcadier-php#-api-documentation)
* [Example Project]()
* [Changelog](https://github.com/Arcadier/arcadier-php#-api-documentation)
* [License](https://github.com/Arcadier/arcadier-php#license)
* [Documentation]()

## Introduction

This PHP SDK is an API wrapper that allows developers to integrate their PHP applications easily with Arcadier's APIs. It does the heavily lifting of building the requests and authentication for every API call

This lets you focus on **building your front-end** instead of having to worry about if you setup the backend properly with our APIs.

All you need to know is how to which parameters in each function. This is documenteted [here]().

## Requirements
* PHP 7.0.0+
* An Arcadier marketplace of [package Scale](https://www.arcadier.com/packages.html) or above (including [Enterprise](https://www.arcadier.com/enterprise/))

## Installation via Composer
You can install SDK via composer using the following command line:
```php
composer require arcadier/test-packagist-sdk
```

## 💡 Getting Started
### Authentication
Once the SDK is installed in your directory, go to `/vendor/arcadier/test-packagist-sdk/sdk/admin_token.php` and `/vendor/arcadier/test-packagist-sdk/sdk/ApiSdk.php` and change the following variables to your marketplace's:

* `$client_id`
* `$client_secret`
* `$marketplace`
* `$protocol` is either https or http depending on your server
* `$marketplace` is your marketplace domain.

`$client_id` and `$client_secret` are found in the Account Settings of you Administrator portal.

### Trying it
In every PHP script you intend to call Arcadier's API, make sure to include the following snippet:
```php
include '../vendor/arcadier/test-packagist-sdk/sdk/ApiSdk.php';
$sdk = new ApiSdk();
```

Get your marketplace's information

**GET** **```/api/v2/marketplaces```** is mapped to `$sdk->getMarketplaceInfo();`
```php
$marketplace_info = $sdk->getMarketplaceInfo();
echo $marketplace_info;
```

Listing all items:

**GET** **```/api/v2/items```** is mapped to `$sdk->getAllItems();`
```php
$item_list = $sdk->getAllItems();
echo $item_list['Records']; //The actual array of items is in the "Records" field of the JSON response
```

Searching for an item

**POST** **```/api/v2/items```** is mapped to `$sdk->getAllItemsJsonFiltering();`
```php
$data = [
    'keywords' => 'string',
    'pagesize' => 'string',
    'Categories' =>[
        'string'
    ],
    'sellerID' => 'string'
];

$response = $sdk->getAllItemsJsonFiltering($data);
$results = $response['Records']; //The actual array of matching items is in the "Records" field of the JSON response
```

Documentation
===

## Table of contents
* [User Accounts & Passwords]()
* [Addresses]()
* [Items]()



## User Accounts
### Get A User's details

**GET** **```/api/v2/users/{{userID}}```** is mapped to `getUserInfo($id, $include)`

Arguments:
* `$id` - *(Required)* User GUID (string) 
* `$includes` - *(Optional)* Takes the following values (string):
  * `"addresses"` 

### Create Buyer Account

**POST** **```api/v2/accounts/register```** is mapped to `registerUser($data)`
```php
$data = [
  'Email' => 'string', //email format
  'Password' => 'string', //at least 6 characters long
  'ConfirmPassword' => 'string'
];
$newUser = $sdk->registerUser($data);
``` 

### Update User information
**PUT** **`/api/v2/users/{{userID}}`** is mapped to `updateUserInfo($id, $data)`

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

### Upgrade User Role
**PUT `/api/v2/admins/{{adminID}}/users/{{userID}}/roles/{{role}}`** is mapped to `upgradeUserRole($id, $role)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$role` - *(Required)* Takes one of the following values (string)
  * "merchant"
  * "Admin"

### Delete User
**DELETE `/api/v2/admins/{{adminID}}/users/{{userID}}`** is mapped to `deleteUser($id)`

Arguments:
* `$id` - *(Required)* User GUID (string)

### Get Password Reset Token
**POST `/api/v2/admins/{{adminID}}/password`** is mapped to `resetPassword($data)`
```php
$data = [
    'UserID': 'string', //User GUID of user to reset password for
    'Action': 'token'
];
$result = $sdk->$resetPassword($data);
```

### Update Password
**PUT `/api/v2/users/{{userID}}/password`** is mapped to `updatePassword($data, $userId)`

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
```

## Addresses
### Get User Address
**GET ``/api/v2/users/{{userID}}/addresses/{{addressID}}``** is mapped to `getUserAddress($id,  $addressID)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$addressID` - *(Optional)* Address GUID (string). Ommitting it will return all addresses of that user.

### Create User Address
**POST ``/api/v2/users/{{userID}}/addresses``** is mapped to `createUserAddress($id, $data)`

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

### Update User Address
**PUT ``/api/v2/users/{{userID}}/addresses/{{addressID}}``** is mapped to `updateUserAddress($id, $addressID, $data)`

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

### Delete User Address
**DELETE ``/api/v2/users/{{userID}}/addresses/{{addressID}}``** is mapped to `deleteUserAddress($id, $addressID)`

Arguments:
* `$id` - *(Required)* User GUID (string)
* `$addressID` - *(Optional)* Address GUID (string). Ommitting it will return all addresses of that user.

## Items
### Get Item Information
**GET `` ``** is mapped to `getItemInfo($id)`

Arguments:
* * `$id` - *(Required)* Item GUID (string)

### Get All Items
**GET** **```/api/v2/items```** is mapped to `getAllItems();`
```php
$item_list = $sdk->getAllItems();
echo $item_list['Records']; //The actual array of items is in the "Records" field of the JSON response
```
---

### Searching for an item

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
**POST ``/api/v2/merchants/{{merchantID}}/items``** is mapped to `createItem($data, $merchantId)`

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

### Create Listing/Booking
**POST ``/api/v2/merchants/{{merchantID}}/items``** is mapped to `createItem($data, $merchantId)`

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
### Edit Item/Listing/Booking
**PUT ``/api/v2/merchants/{{merchantID}}/items/{{itemID}}``** is mapped to `editItem($data, $merchantId, $itemId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$itemId` - *(Required)* Item GUID (string)
* `$data` - Item details (Object)

Documentation and `$data` details can be found [here](https://apiv2.arcadier.com/#8af9bf27-a3fb-4623-b8d0-f53a67697c47).


### Delete Item/Listing/Booking
**DELETE ``/api/v2/merchants/{{merchantID}}/items/{{itemID}}``** is mapped to `deleteItem($merchantId, $itemId)`

Arguments:
* `$merchantId` - *(Required)* Merchant GUID (string)
* `$itemId` - *(Required)* Item GUID (string)

### Tag Item/Listing/Booking
**POST ``/api/v2/merchants/{{merchantID}}/items/{{itemID}}/tags``** is mapped to `tagItem($data, $merchantId, $itemId)`

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

### Get Item Tags
**GET ``/api/v2/tags``** is mapped to `getItemTags($pageSize = null, $pageNumber = null)`

Arguments:
* `$pageSize` - *(Optional)* Number of results per page (integer)
* `$pageNumber` - *(Optional)* Page number (integer)

More about pagination [here](https://apiv2.arcadier.com/#pagination)

### Delete Tags
**DELETE ``/api/v2/tags``** is mapped to `deleteTags($data)`

```php
$data = [
    'string',
    'string'
];
```


## Changelog

## License
