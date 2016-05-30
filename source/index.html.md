---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Xerts REST API.

This API is designed to work with the Xerts app and also allow 3rd party developers access to the Xerts coupon system.

## User Flow

The API is designed with a specific user-flow in mind. When a user walks into a store with his/her app, they are presented with an NFC device to tap at the point of sale (POS). When tapping their device to the NFC POS device, a web address with a special unique identifier (Site ID) is relayed to the user's device. The user's device then receives access to a range of 'coupons' for that POS, which are redeemable on the spot. 

Staff at the site can manually redeem the coupon with an interface behind the counter. This is done via a 5-digit redeem code shown on the user's device. The communication and validation process may change in the future, however this is the MVP method chosen.

[![User flow diagram](images/xerts-user-diagram.png)](images/xerts-user-diagram.png)


# Endpoints
###### Last updated: 30 May 2016

## Create a User

<aside class="success">
Post parameters in JSON format in the body of the request: `{ username: ..., email: ..., etc. }`
</aside>

> Use this command:

```shell
curl -X POST hostname/api/members -d "{
  'phone': 'string',
  'type': 'string',
  'fname': 'string',
  'lname': 'string',
  'username': 'string',
  'password': 'string',
  'email': 'string',
  'emailVerified': bool,
  'status': 'string'
}"
```

> The above command returns JSON structured like this:

```json
{
  "id": "d426a9c0-22fe-11e6-96a3-bdf7343ebeee",
  "phone": "023584620",
  "type": "owner",
  "fname": "Micky",
  "lname": "Blue",
  "username": "mblurulez",
  "email": "bluezey@gmail.com",
  "status": "active"
}
```

This endpoint creates a user.

### HTTP Request

`POST http://dev.xerts.io/api/members`

### Body Parameters

Parameter | Required | Type | ID | Description
----------|----------|------|----|--------------------------------------
username         |N       |String|N |Conforms to User model              
email |Y       |String|N |Conforms to User model                    
password |Y       |String|N |Conforms to User model                   
phone |N       |String|N |User contact Phone                    
type |Y       |String|N |Type can be: [vendor_admin, site_admin]                               
fname |N       |String|N |First Name                               
lname |N       |String|N |Last Name                               

## Log a user in

This endpoint logs a user in.

> Use this command:

```shell
curl -X POST hostname/api/members/login -d "{'email': 'string', 'password': 'string'}"
```

> The above command returns JSON structured like this:

```json
{
  "id": "QvS1bIt6WkhA6rCZXpUDnFnzszaU0WZBW0RWNtEMCqGZ4iOZ2Mc9EFKJsTmt1czU",
  "ttl": 1209600,
  "created": "2016-05-26T05:17:55.231Z",
  "userId": "d426a9c0-22fe-11e6-96a3-bdf7343ebeee"
}
```
> The `id` is an authentication token. Use this token in request headers: `{ Authentication: QvS1bIt6WkhA6rCZXpUDnFnzszaU0WZBW0RWNtEMCqGZ4iOZ2Mc9EFKJsTmt1czU }`

### HTTP Request

`POST http://dev.xerts.io/api/members/login`

### Body Parameters

Parameter | Required | Type | ID | Description
----------|----------|------|----|--------------------------------------
email |Y       |String|N | user's email address                    
password |Y       |String|N | users's password                   

## Create a Vendor for User

> Use this command:

```shell
curl -X POST hostname/api/members/:id/vendors -d "{
    'vendor_name': 'string',
    'head_office_address1': 'string',
    'head_office_address2': 'string',
    'head_office_zipcode': 'string',
    'head_office_city': 'string',
    'head_office_country': 'string',
    'head_office_phone': 'string',
    'description': 'string'
  }"
```

> The above command returns JSON structured like this:

```json
{
  "id": "d426a9c0-22fe-11e6-96a3-bdf7343ebeee",
  "phone": "023584620",
  "type": "owner",
  "fname": "Micky",
  "lname": "Blue",
  "username": "mblurulez",
  "email": "bluezey@gmail.com",
  "status": "active"
}
```

This endpoint creates a vendor for user.

### HTTP Request

`POST http://dev.xerts.io/api/members/:id/vendors`

<aside class="notice">
`id` is always a GUID. Replace this with the member with `id` you are creating a
vendor for.
</aside>

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|--------------------------------------
 vendor_name         |Y       |String|N |Name of the organisation              
 head_office_address1|Y       |String|N |Address First line                    
 head_office_address2|N       |String|N |Address Second line                   
 head_office_address3|N       |String|N |Address Third line                    
 head_office_zipcode |Y       |String|N |Zipcode                               
 head_office_city    |Y       |String|N |City / Province                       
 head_office_country |Y       |String|N |Country                               
 head_office_phone   |N       |String|N |Contact Phone number                  
 description         |Y       |String|N |Address First line

## Create a Site for a Vendor

> Use this command:

```shell
curl -X POST hostname/api/vendors/:id/sites -d "{
      'title': 'string',
      'address': 'string',
      'gps': {
        'lat': 'float',
        'lng': 'float'
      }"
```

> The above command returns JSON structured like this:

```json
{
  "id": "d426a9c0-22fe-11e6-96a3-bdf7343ebeee",
  "title": "Whal Burger NY East",
  "address": "34 22nd East. St.",
  "gps": {
    "lat": "101.890",
    "lng": "22.456"
  }
}
```

This endpoint creates a site for a vendor.

### HTTP Request

`POST http://dev.xerts.io/api/vendors/:id/sites`

<aside class="notice">
`id` is always a GUID. Replace this with the member with `id` you are creating a
vendor for.
</aside>

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------
 title  |Y       |String|N |Site title (Branch/Franchise Location)
 address|N       |String|N |If an address is available, add it here
 gps    |N       |String|N |If GPS position is available, add it here { lat: 'float', long: 'float' }

## Create a User owned by a Vendor

> Use this command:

```shell
curl -X POST hostname/api/vendors/:id/members -d "{
  'phone': 'string',
  'type': 'string',
  'fname': 'string',
  'lname': 'string',
  'username': 'string',
  'password': 'string',
  'email': 'string',
  'emailVerified': bool,
  'status': 'string'
}"
```

> The above command returns JSON structured like this:

```json
{
  "id": "d426a9c0-22fe-11e6-96a3-bdf7343ebeee",
  "phone": "023584620",
  "type": "owner",
  "fname": "Micky",
  "lname": "Blue",
  "username": "mblurulez",
  "email": "bluezey@gmail.com",
  "status": "active"
}
```

This endpoint creates a user that is owned by a vendor. This user can be assigned as admin of one of the vendor's sites, or as a vendor owner.

### HTTP Request

`POST http://dev.xerts.io/api/vendors/:id/members`

<aside class="notice">
`id` is always a GUID. Replace this with the member with `id` you are creating a
vendor for.
</aside>

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------
username         |N       |String|N |Conforms to User model              
email |Y       |String|N |Conforms to User model                    
password |Y       |String|N |Conforms to User model                   
phone |N       |String|N |User contact Phone                    
type |Y       |String|N |Type can be: [vendor_admin, site_admin]                               
fname |N       |String|N |First Name                               
lname |N       |String|N |Last Name  

## Add a Site Admin

> Use this command:

```shell
curl -H "Authorization: <ACCESS_TOKEN>" -X PUT hostname/api/sites/:id/admin -d "{
  'adminId': 'string'
}"
```

> The above command returns JSON structured like this:

```json
{
  "results": {
    "id": "74651f00-22e8-11e6-953c-b1e050c9762d",
    "title": "Danielhaven",
    "address": "069 Jayne Vista",
    "gps": {
      "lat": 11.78,
      "lng": 83.0154
    },
    "vendorId": "7461eab0-22e8-11e6-953c-b1e050c9762d",
    "adminId": "d426a9c0-22fe-11e6-96a3-bdf7343ebeee"
  }
}
```

This endpoint adds a user to a site. User must be owned by the vendor that owns the site.

### HTTP Request

`POST http://dev.xerts.io/api/sites/:id/admin`

<aside class="notice">
`id` is always a GUID. Replace this with the member with `id` you are creating a
vendor for.
</aside>

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------
id         |Y       |String|Y |The ID of the site (embed in URL)
adminId         |Y       |String|Y |The ID of the user you want to set as admin of this site.

## Create an Offer for a Vendor

> Use this command:

```shell
curl -H "Authorization: <ACCESS_TOKEN>" -X POST hostname/api/vendors/:id/offers -d "{
  'title': 'string',
  'description': 'string',
  'style_bordercolor': 'string',
  'style_backgroundcolor': 'string',
  'feature_image': 'string'
}"
```

> The above command returns JSON structured like this:

```json
{
  "id": "58b52db0-22e6-11e6-b8de-1b32b0fa4bd3",
  "title": "West Bufordview",
  "description": "Illo aliquid eos velit quod. Eveniet facere vitae officia. Aliquam veniam iste. Adipisci laboriosam rerum eum ad ut molestiae hic. Doloremque et sit beatae reiciendis nisi reprehenderit eum ut ea.\n \rId voluptas quia non sunt distinctio. Molestiae alias maxime veritatis blanditiis vitae voluptas vitae non maiores. Nam cupiditate voluptatibus est aut. Qui repudiandae reiciendis in cumque dicta amet ipsum.\n \rConsequatur et magnam. Dicta corporis quasi sint distinctio neque et quis et. Quo nemo iste eveniet quibusdam suscipit velit sed.",
  "style_bordercolor": "mint green",
  "style_backgroundcolor": "blue",
  "feature_image": "string",
  "vendorId": "5887b510-22e6-11e6-b8de-1b32b0fa4bd3"
}
```

This endpoint adds an offer for a vendor. Vendor must be owned by logged in user to work.

### HTTP Request

`POST http://dev.xerts.io/api/vendor/:id/offers`

<aside class="notice">
`id` is always a GUID. Replace this with the member with `id` you are creating a
vendor for.
</aside>

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------
title         |Y       |String|N |The site title
description         |Y       |String|N |Description of the site
style_bordercolor         |Y       |String|N |Border color styling
style_backgroundcolor         |Y       |String|N |Background color styling
feature_imagae         |Y       |String|N |Feature image for offer/coupon.
vendorId         |Y       |String|N |Owning vendor ID

## Add an Offer to a Site

> Use this command:

```shell
curl -H "Authorization: <ACCESS_TOKEN>" -X PUT hostname/api/offers/:id/sites/rel/:siteId -d "{
  'instance_cost': 0.12,
  'instance_limit': 1000,
  'instance_issues': 3,
  'total_cost': 120.00,
  'instance_expiary_date': '2017-05-26'
}"
```

> The above command returns JSON structured like this:

```json
{
  "id": "748ec710-22e8-11e6-953c-b1e050c9762d",
  "instance_cost": 0.12,
  "instance_limit": 1010,
  "instance_issues": 1,
  "total_cost": 121.19999999999999,
  "instance_expiary_date": "2017-05-26",
  "offerId": "748d1960-22e8-11e6-953c-b1e050c9762d",
  "siteId": "74651f00-22e8-11e6-953c-b1e050c9762d"
}
```

This endpoint assigns an offer to a site. If an offer is assigned to a site, clients can receive coupons based on those offers at their sites.

### HTTP Request

`PUT http://dev.xerts.io/api/offers/:id/sites/rel/:siteId`

<aside class="notice">
`id` and `siteId` are always a GUID.
</aside>

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------
instance_cost         |Y       |String|N |Cost per coupon
instance_limit         |Y       |String|N |Maximum number of coupons to be issued
instance_issues         |Y       |String|N |Number of coupons been issued
total_cost         |Y       |String|N |Total cost of campaign, given instance_cost and instance_limit
instance_start_issue_date         |N       |String|N |Time of first coupon issue
instance_last_issue_date         |N       |String|N |Last time a coupon was issued
instance_expiary_date         |N       |String|N |Expiary date of offer at site

## Add a Client Device

> Use this command:

```shell
curl -X POST hostname/api/device/ -d "{
  'id': 'xxxx-xxxx-xxxx-xxxxx',
  'device_model': 'iPhone 6s',
  'device_os': 'iOS 9.3'
}"
```

> The above command returns JSON structured like this:

```json
{
  "id": "89611c2c-f016-4e3b-8078-6abe40550552",
  "device_model": "iPhone 6s",
  "device_os": "iOS 9.3"
}
```

This endpoint creates a client device in the system. At this time there are no users associated devices, but this will change in future versions.

### HTTP Request

`POST http://dev.xerts.io/api/devices`

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------
id         |Y       |String|Y |A unique identifier for the device will be posted to the API, generated by the client device it's self.
device_model         |N       |String|N |Device model eg. iPhone 7
device_os         |N       |String|N |The operating system and version number.

## Create Coupons for Device at Site

> Use this command:

```shell
curl -X POST hostname/api/siteOffers/:siteId/device/:deviceId
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "id": 2,
      "redeem_code": "Q70Q7",
      "deviceId": "89611c2c-f016-4e3b-8078-6abe40550552",
      "siteOfferId": "748ec710-22e8-11e6-953c-b1e050c9762d",
      "offer": {
        "id": "748d1960-22e8-11e6-953c-b1e050c9762d",
        "title": "Port Francis",
        "description": "Ut sint qui sint. Corrupti autem incidunt iure nisi. At nulla deserunt saepe odio et esse adipisci. In harum ipsum rerum dolor est illo numquam et.\n \rMinus aut laboriosam accusantium dolores. Blanditiis assumenda eaque voluptas fuga fugit eius ipsum labore est. Non debitis est suscipit consectetur aut. Labore est vitae sed et odio quas explicabo ipsa qui. Perferendis vel aut.\n \rSed voluptatem temporibus. Aut voluptatibus voluptates qui aliquid hic et iusto quis. Eos sit nesciunt quod laudantium aspernatur sequi earum. Et occaecati eos cupiditate minima et. Facilis quaerat excepturi quod at eum quo neque consequuntur aliquid. Id culpa non.",
        "style_bordercolor": "grey",
        "style_backgroundcolor": "salmon",
        "feature_image": "string",
        "vendorId": "7461eab0-22e8-11e6-953c-b1e050c9762d"
      }
    }
  ]
}
```

> If the siteId does not exist, JSON error is returned:

```json
{
  "error": {
    "status": 404,
    "message": "Didnt return anything."
  }
}
```

This generates coupons.

### HTTP Request

`POST http://dev.xerts.io/api/siteOffers/:siteId/device/:deviceId`

### Body Parameters

 Parameter | Required | Type | ID | Description
----------|----------|------|----|-------------------------------


## Get all/site Coupons for Device

> Use this command to get every coupon for device:

```shell
curl -X POST hostname/api/coupons/site/all/device/:deviceId
```

> Use this command to get only site-specific coupons for device:

```shell
curl -X POST hostname/api/coupons/site/:siteId/devices/:deviceId
```

> The above commands return JSON structured like this:

```json
{
  "results": [
    {
      "id": 10,
      "redeem_code": "9CJBG",
      "deviceId": "255e294a-34dc-4a4e-a80a-d4bb98890e5c",
      "siteOfferId": "d634c6d0-25ff-11e6-b75e-7bf1c637b078",
      "offer": {
        "id": "d62fe4d0-25ff-11e6-b75e-7bf1c637b078",
        "title": "Port Hal",
        "description": "Perferendis nihil dicta suscipit. Aperiam ex quibusdam error sit molestias quo dolor veniam deleniti. Natus magni molestiae at sint nihil excepturi incidunt. Sequi nobis molestiae asperiores laboriosam suscipit autem est. Quidem eos vitae nesciunt fugit qui sunt. Minus ex alias.\n \rAd maxime sint quam blanditiis. Consequatur tempore aut quia assumenda ea placeat veritatis consequuntur. Quibusdam sed facilis suscipit velit aut voluptate. Quidem est eveniet dignissimos similique laborum dolores. Eos ut quod facere numquam nisi quo ut eum aliquam.\n \rEt corrupti iure facilis possimus repudiandae doloremque omnis repellendus harum. Omnis et impedit tenetur. Fugit quisquam consequatur veniam non nemo id est. Quia id est voluptas et et quia. Occaecati consequuntur ut eaque inventore dolorem distinctio quis aliquam. A temporibus odit ut placeat natus aut ipsam voluptate.",
        "style_bordercolor": "sky blue",
        "style_backgroundcolor": "magenta",
        "feature_image": "string",
        "vendorId": "d6055260-25ff-11e6-b75e-7bf1c637b078"
      }
    },
    {
      "id": 11,
      "redeem_code": "AO5XW",
      "deviceId": "255e294a-34dc-4a4e-a80a-d4bb98890e5c",
      "siteOfferId": "6ead4970-25fe-11e6-9f99-1520fe63256a",
      "offer": {
        "id": "6eab9bc0-25fe-11e6-9f99-1520fe63256a",
        "title": "Millershire",
        "description": "Facilis esse aliquid culpa molestiae illo consectetur animi expedita voluptatum. Exercitationem illum quaerat voluptate laboriosam deserunt commodi provident iure dolorum. Sapiente itaque veritatis.\n \rAb eaque laboriosam. Autem error corporis ullam. A eaque vel perspiciatis maxime aut eos quia enim ea.\n \rEst quia natus. Aperiam modi voluptatem. Quaerat sint enim quas quis dignissimos est. Aliquam nemo et. Ea cupiditate similique aut aut quos eos.",
        "style_bordercolor": "maroon",
        "style_backgroundcolor": "lime",
        "feature_image": "string",
        "vendorId": "6e7fa9c0-25fe-11e6-9f99-1520fe63256a"
      }
    }
  ]
}
```

This gets all existing coupons for the device. Be sure to use the `all`, otherwise you should use a `siteId` to get all site-specific coupons for that device.

<aside class="notice">
Remember, `siteId` is a GUID, and should look something like this: 03548350-25fe-11e6-b93a-dd966e070e71
</aside>

### HTTP Request

`GET http://dev.xerts.io/api/coupons/sites/all/devices/:deviceId`
`GET http://dev.xerts.io/api/coupons/sites/:siteId/devices/:deviceId`

### Body Parameters

 Parameter | Required | Type | ID | Description
-----------|----------|------|----|-------------------------------
None.      |

## Redeem a coupon at a site

> Use this command to get only site-specific coupons for device:

```shell
curl -X POST hostname/api/sites/:siteId/
```

> The above commands return JSON structured like this:

```json
{
  "results": [
    {
      "id": 36,
      "redeem_code": "D068Y",
      "redeemed_date": "2016-05-30T06:54:36.000Z",
      "deviceId": "656e6b6d-6a69-4e48-b66c-b917d3561ef0",
      "siteOfferId": "5f750170-2633-11e6-a10f-a197e0a3258d",
      "offer": {
        "id": "5f732cb0-2633-11e6-a10f-a197e0a3258d",
        "title": "North Lucileton",
        "description": "Quia aut unde quas consequuntur qui corporis maxime. Eum expedita consequatur dolore veritatis doloribus ullam perspiciatis. Aliquam aut sint atque. Quas autem officia et tempore sit reprehenderit veritatis dignissimos quidem.\n \rIpsa excepturi numquam nemo. Distinctio ad non est quo atque consequatur. Provident nihil accusantium qui. Necessitatibus soluta suscipit nobis commodi nihil deserunt sint corrupti recusandae. Itaque et et voluptas perspiciatis impedit quas quae aut.\n \rCommodi aspernatur provident odio beatae temporibus nam libero. Nihil enim voluptatum nostrum unde. Itaque velit consequuntur. Voluptas atque doloribus eligendi. Qui minima velit voluptates.",
        "style_bordercolor": "mint green",
        "style_backgroundcolor": "azure",
        "feature_image": "string",
        "vendorId": "5f440660-2633-11e6-a10f-a197e0a3258d"
      }
    }
  ]
}
```

This endpoint gets all coupons for site with `siteId`.

<aside class="notice">
Remember, `siteId` is a GUID, and should look something like this: 03548350-25fe-11e6-b93a-dd966e070e71
</aside>

### HTTP Request

`GET http://dev.xerts.io/api/sites/:siteId/coupons`

### Body Parameters

 Parameter | Required | Type | ID | Description
-----------|----------|------|----|-------------------------------
None.      |

## Redeem a coupon at a site

> Use this command to get only site-specific coupons for device:

```shell
curl -X POST hostname/api/coupons/sites/:siteId/redeem/:code
```

> The above commands return JSON structured like this:

```json
{
  "results": {
    "id": 19,
    "redeem_code": "B5Z7E",
    "redeemed_date": "2016-05-30T03:14:18.000Z",
    "deviceId": "60ef5b5e-7229-4758-8184-da613de405a9",
    "siteOfferId": "0583f660-2612-11e6-9206-cd9a11b559ba"
  }
}
```

This endpoint redeems a coupon with redeem code `:code` at site with id `siteId`.

<aside class="notice">
Remember, `siteId` is a GUID, and should look something like this: 03548350-25fe-11e6-b93a-dd966e070e71
</aside>

### HTTP Request

`GET http://dev.xerts.io/api/coupons/sites/:siteId/redeem/:code`

### Body Parameters

 Parameter | Required | Type | ID | Description
-----------|----------|------|----|-------------------------------
None.      |
