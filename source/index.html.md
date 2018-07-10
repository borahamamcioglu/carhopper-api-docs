---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell - cURL

toc_footers:
  - <a href='mailto:support@carhopper.co?subject=Carhopper API Access Request'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the CarHopper API! You can use it to search, review and book luxury cars.
You can view Shell code examples in the dark area to the right.

# Authentication

## Authorize API call


```shell
# With shell, you can just pass the correct header with each request
curl "https://carhopper.co/api/v1/some/endpoint"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> Make sure to replace `YOUR_API_KEY` with your API key.

CarHopper expects the API key to be included in a header, for all API requests to the server.

`Authorization: Bearer YOUR_API_KEY`

You can get a CarHopper API key from your CarHopper profile page. Be sure to use "Copy Link" in order to have it coppied the right way.


## Authorize Carhopper Customer


```shell
# With shell, you can just pass the correct header with each request
curl "https://carhopper.co/api/v1/some/endpoint"
  -H "Authorization: Bearer YOUR_API_KEY"
  -H "Customer-Token: CUSTOMER_TOKEN"
```

> Make sure to replace `YOUR_API_KEY` with your API key, and `CUSTOMER_TOKEN` with your customer API key.

Some CarHopper API actions require existing user to be authorized. You can get a CarHopper `CUSTOMER_TOKEN` from the customer's very first <a href="create">Reservation Create endpoint</a> on Carhopper (if the account with the same email doesn't exist). You can store it within your users database for future use.

If your user is already a Carhopper Customer, you can prompt him to login with API <a href="#customer-login">Customer Login</a> endpoint.

CarHopper expects the Customer Token heared to be included in every API request that requires customer authentication.

`Customer-Token: CUSTOMER_TOKEN`


## Customer Login


```shell
# With shell, you can just pass the correct header with each request
curl "https://carhopper.co/api/v1/login"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> Make sure to replace `YOUR_API_KEY` with your API key.

Use this endpoint to authorize existing Carhopper Customer and get the `CUSTOMER_TOKEN`. 


### HTTP Request

`GET https://carhopper.co/api/v1/login`


### Query Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
email | string | Carhopper Customer's email 
password | string | Carhopper Customer's password



# Reservation

## Estimate

```shell
curl "https://carhopper.co/api/v1/reservation/estimate"
  -H "Authorization: Bearer YOUR_API_KEY"
  -H "Customer-Token: CUSTOMER_TOKEN"
```
> The above command returns JSON structured like this:

```json
{
    "status": 1,
    "message": "",
    "data": {
        "days": 30,
        "price": 2700,
        "extras": 0,
        "delivery": 20,
        "insurance": 40,
        "service_fee": 0,
        "surcharge": 0,
        "energy_recovery_fee": 0,
        "discount": 270,
        "discount_percent": 10,
        "referral_discount": 0,
        "total": 2490
    }
}
```

This endpoint estimates car reservation for given dates. 
If `country` is not USA, CDW insurance will be applyed.
This endpoint requires `CUSTOMER_TOKEN`.


### HTTP Request

`GET https://carhopper.co/api/v1/reservation/create?start_at=20180910&end_at=20181010&car_id=1255&country=FR`


### Query Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
startDate | Date [YYYY-MM-DD] | Start date of rental 
endDate | Date [YYYY-MM-DD] | End date of rental
country | ISO 2-letter abbrevation | Renter's Licence Country


## Create

```shell
curl "https://carhopper.co/api/v1/reservation/create"
  -H "Authorization: Bearer YOUR_API_KEY"
  -H "Customer-Token: CUSTOMER_TOKEN"
```
> The above command returns JSON structured like this:

```json
{
	"status": 1,
	"message": "Reservation created",
	"data": {
		"reservation_id": 873,
		"checkout_url": "https://carhopper.co/embed/checkout/9CcjNTpljPbWSEHfPcrEwHCNQnHtySt2HmkEXHyXtHXEWGQwip8SW3ilRg9M/873",
		"customer_token": "9CcjNTpljPbWSEHfPcrEwHCNQnHtySt2HmkEXHyXtHXEWGQwip8SW3ilRg9M"
	}
}
```

> or returns JSON structured like this in case of error:

```json
{
   "status":0,
   "message":"Bad request.",
   "data":[
      "The start at field is required.",
      "The end at field is required.",
      "The car id field is required.",
      "The email field is required."
   ]
}
```

This endpoint creates car reservation. 

In case that user is new to the Carhopper system, a new one is created and `customer_token` will be in response. You can check in <a href="authorize-carhopper-customer">here</a> how to use it. If you already have `CUSTOMER_TOKEN`, include it in header of request.

If `country` is not USA - CDW insurance will be applyed. 
If `country` is USA - renter's `insurance_policy`, `insurance_company_name` and `license_state` are required.


### HTTP Request

`GET https://carhopper.co/api/v1/reservation/create`


### Query Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
start_at | Date [YYYY-MM-DD] | Start date of rental 
end_at | Date [YYYY-MM-DD] | End date of rental
car_id | int | ID of the car obtained from search results.
email | string  | Contact email of the renter.
firstname | string | Firt name of the renter 
lastname | string | Last name of the renter
birthday | Date [YYYY-MM-DD] | Birthdate of the renter
phone | string | Phone number of the renter.
country | ISO 2-letter abbrevation | Renter's Licence Country
state | ISO 2-letter abbrevation | Renter's Licence State if Country is US
license_number | int | License number of the renter
license_state | string | License state of the renter
insurance_policy | string | Insurance Policy of the renter
insurance_company_name | string | Insurance Company of the renter
delivery_address | string | Delivery address


### Return Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
reservation_id | int | ID of created Reservation
checkout_url | string | Reservation Checkout URL
customer_token | string | Carhopper Customer Token (optional)


###Payment embed form

> Redirect URL with appended return URL parameter:

```shell
"https://carhopper.co/embed/checkout/[SOME_HASH]/873?return_url=https%3A%2F%2Fexample.com%2Fthankyou"
```

Reservation Checkout URL will be provided in `checkout_url` as a part of the response. Customer shoould be redirected to this URL in order to pay for the reservation. Once the payment is done, canceled or failed, customer will be redirected to `return_url` you provided in the request to our embed payment form. You must append this parameter to `checkout_url` when you are creating the request, and it needs to be URL encoded.

## Get

```shell
curl "https://carhopper.co/api/v1/reservation/115"
  -H "Authorization: Bearer YOUR_API_KEY"
  -H "Customer-Token: CUSTOMER_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
    "id": 1508,
    "user_id": 2783,
    "car_id": 1379,
    "contact_email": "customer@carhopper.co",
    "price": "180.00",
    "days": 1,
    "discount": "18.00",
    "total_price": "162.00",
    "insurance": {
        "details": [],
        "total": 0
    },
    "extras": [],
    "start_at": {
        "date": "2018-08-10 00:00:00.000000",
        "timezone_type": 3,
        "timezone": "US/Eastern"
    },
    "end_at": {
        "date": "2018-08-11 00:00:00.000000",
        "timezone_type": 3,
        "timezone": "US/Eastern"
    },
    "created_at": {
        "date": "2018-06-19 10:06:13.000000",
        "timezone_type": 3,
        "timezone": "US/Eastern"
    },
    "responded_at": null,
    "deleted_at": null,
    "status": "hold",
    "referral_discount": "0.00",
    "delivery_address": null,
    "coupon": {
        "id": 365,
        "discount_percent": 10,
        "discount_amount": "0.00"
    }
}
```

Get the user's existing reservation.


### HTTP Request

`GET https://carhopper.co/api/v1/reservation/<ID>`


### Query Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
reservation_id | int | Reservation ID

## Get all

```shell
curl "https://carhopper.co/api/v1/reservations"
  -H "Authorization: Bearer YOUR_API_KEY"
  -H "Customer-Token: CUSTOMER_TOKEN"
```

> The above command returns JSON structured like this:

```json
[
	{
	    "id": 1508,
	    "user_id": 2783,
	    "car_id": 1379,
	    "contact_email": "customer@carhopper.co",
	    "price": "180.00",
	    "days": 1,
	    "discount": "18.00",
	    "total_price": "162.00",
	    "insurance": {
	        "details": [],
	        "total": 0
	    },
	    "extras": [],
	    "start_at": {
	        "date": "2018-08-10 00:00:00.000000",
	        "timezone_type": 3,
	        "timezone": "US/Eastern"
	    },
	    "end_at": {
	        "date": "2018-08-11 00:00:00.000000",
	        "timezone_type": 3,
	        "timezone": "US/Eastern"
	    },
	    "created_at": {
	        "date": "2018-06-19 10:06:13.000000",
	        "timezone_type": 3,
	        "timezone": "US/Eastern"
	    },
	    "responded_at": null,
	    "deleted_at": null,
	    "status": "hold",
	    "referral_discount": "0.00",
	    "delivery_address": null,
	    "coupon": {
	        "id": 365,
	        "discount_percent": 10,
	        "discount_amount": "0.00"
	    }
	},
	{
	    "id": 1509,
	    "user_id": 2783,
	    "car_id": 1380,
	    "contact_email": "customer@carhopper.co",
	    "price": "180.00",
	    "days": 1,
	    "discount": "18.00",
	    "total_price": "162.00",
	    "insurance": {
	        "details": [],
	        "total": 0
	    },
	    "extras": [],
	    "start_at": {
	        "date": "2018-08-10 00:00:00.000000",
	        "timezone_type": 3,
	        "timezone": "US/Eastern"
	    },
	    "end_at": {
	        "date": "2018-08-11 00:00:00.000000",
	        "timezone_type": 3,
	        "timezone": "US/Eastern"
	    },
	    "created_at": {
	        "date": "2018-06-19 10:06:13.000000",
	        "timezone_type": 3,
	        "timezone": "US/Eastern"
	    },
	    "responded_at": null,
	    "deleted_at": null,
	    "status": "hold",
	    "referral_discount": "0.00",
	    "delivery_address": null,
	    "coupon": {
	        "id": 365,
	        "discount_percent": 10,
	        "discount_amount": "0.00"
	    }
	}
]
```

Get the customer's pending reservations.


### HTTP Request

`GET https://carhopper.co/api/v1/reservations`


## Cancel

```shell
curl "https://carhopper.co/api/v1/cancel/5"
  -H "Authorization: Bearer YOUR_API_KEY"
  -H "Customer-Token: CUSTOMER_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
	"status": 1,
	"message": "Reservation cancelled"
}
```

> or returns JSON structured like this in case of error:

```json
{
	"status": 0,
	"message": "Reservation doesn't exist"
}
```

This endpoint cancels the previously created reservation.


### HTTP Request

`GET https://carhopper.co/api/v1/reservation/cancel/<ID>`


### Query Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
reservation_id | int | Reservation ID


# Search

## Search Cars


```shell
curl "https://carhopper.co/api/v1/reservation/search"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"category_counter": {
		"Sedan": 1,
		"Convertible": 2
	},
	"make_counter": {
		"Rolls-royce": 3,
		"Mclaren": 1,
		"Lamborghini": 4,
		"Ferrari": 8,
		"Bentley": 5,
		"Bmw": 14,
		"Mercedes-benz": 16,
		"Porsche": 17,
		"Land rover": 5,
		"Jaguar": 3,
		"Maserati": 2,
		"Audi": 1
	},
	"cars": [{
	    "id": 1379,
	    "avatar": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
	    "longitude": -118.39271329999997,
	    "latitude": 33.9460263,
	    "state_code": "CA",
	    "city": "Los Angeles",
	    "state": "California",
	    "country": "United States",
	    "zip_code": "90045",
	    "address": "6151 W Century Blvd, Los Angeles, CA 90045, USA",
	    "category": "SUV",
	    "gps": 0,
	    "all_wheel_drive": 0,
	    "audio_input": 1,
	    "pet_friendly": 0,
	    "sunroof": 1,
	    "bike_rack": 0,
	    "convertible": 0,
	    "parking_aid": 1,
	    "description": " ",
	    "color": "White",
	    "transmission_type": "Automatic",
	    "daily_price": "150.00",
	    "extra_sunpass_price": null,
	    "extra_childseat_price": null,
	    "extra_gps_price": null,
	    "extra_delivery_price": "40.00",
	    "insurance_cdw_price": "30.00",
	    "insurance_sli_price": null,
	    "insurance_pai_price": null,
	    "insurance_pec_price": null,
	    "insurance_rcp_price": null,
	    "deposit": 0,
	    "milage_limit": 150,
	    "extra_milage": "0.35",
	    "extra_promotion": null,
	    "model_make_id": "Bmw",
	    "model_name": "X3",
	    "model_year": 2017,
	    "model_trim": "",
	    "model_seats": null,
	    "model_doors": null,
	    "images": [
	        {
	            "title": "Bmw X3 2017 White",
	            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-9vodFLMxvn.JPG",
	            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
	            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-QGGtSEFWl7.JPG"
	        },
	        {
	            "title": "Bmw X3 2017 White",
	            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-IYwEJlhImo.JPG",
	            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-3yY6FetBLH.JPG",
	            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-4HdU3XkFhJ.JPG"
	        },
	        {
	            "title": "Bmw X3 2017 White",
	            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-rA2AXGgMB3.JPG",
	            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-JE0K3QpIwD.JPG",
	            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-t0StfY0qUI.JPG"
	        },
	        {
	            "title": "Bmw X3 2017 White",
	            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-SQSF1gt16R.JPG",
	            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-kvzHvFHMtQ.JPG",
	            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-TnZ5BUJ5n7.JPG"
	        }
	    ],
	    "location": {
	        "id": 5,
	        "name": "Los Angeles, CA"
	    },
	    "area": {
	        "id": 10,
	        "name": "Airport proximity"
	    }
	}],
	"totalPageCount": 1
}
```

This endpoint retrieves all cars.


### HTTP Request

`GET https://carhopper.co/api/v1/reservation/search`


### Query Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
startDate | Date [YYYY-MM-DD] | Start date of rental 
endDate | Date [YYYY-MM-DD] | End date of rental
make | string [] | Filter cars by their makes
category | string [] | Filter cars by their categories
location | int | Filter cars by their location 
area | int | Filter cars by their area
page | int | Results pager
orderBy | string | Order results by `priceAsc, priceDesc, distanceAsc`



## Locations

```shell
curl "https://carhopper.co/api/v1/locations"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> The above command returns JSON structured like this:

```json
[{
	"id": 1,
	"name": "Miami, FL"
},
{
	"id": 3,
	"name": "Orlando, FL"
},
{
	"id": 5,
	"name": "Los Angeles"
}]
```

This endpoint retrieves a CarHopper registered car locations.
You can use location `id` to filter cars or get related areas in <a href="search">Search Endpoint</a>.


### HTTP Request

`GET https://carhopper.co/api/v1/locations`


### URL Parameters

No parameters required



## Areas

```shell
curl "https://carhopper.co/api/v1/areas?location_id=3"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> The above command returns JSON structured like this:

```json
[{
	"id": 5,
	"name": "Manhattan",
	"location_id": 3
}]
```

This endpoint retrieves areas related to given `location_id`.
You can use area `id` to filter cars in <a href="search">Search Endpoint</a>.


### HTTP Request

`GET https://carhopper.co/api/v1/areas`


### URL Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
<ID> | int | The ID of the location that areas related to.



## Car Details

```shell
curl "https://carhopper.co/api/v1/car/350"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> The above command returns JSON structrued like this:

```json
{
    "id": 1379,
    "avatar": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
    "longitude": -118.39271329999997,
    "latitude": 33.9460263,
    "state_code": "CA",
    "city": "Los Angeles",
    "state": "California",
    "country": "United States",
    "zip_code": "90045",
    "address": "6151 W Century Blvd, Los Angeles, CA 90045, USA",
    "category": "SUV",
    "gps": 0,
    "all_wheel_drive": 0,
    "audio_input": 1,
    "pet_friendly": 0,
    "sunroof": 1,
    "bike_rack": 0,
    "convertible": 0,
    "parking_aid": 1,
    "description": " ",
    "color": "White",
    "transmission_type": "Automatic",
    "daily_price": "150.00",
    "extra_sunpass_price": null,
    "extra_childseat_price": null,
    "extra_gps_price": null,
    "extra_delivery_price": "40.00",
    "insurance_cdw_price": "30.00",
    "insurance_sli_price": null,
    "insurance_pai_price": null,
    "insurance_pec_price": null,
    "insurance_rcp_price": null,
    "deposit": 0,
    "milage_limit": 150,
    "extra_milage": "0.35",
    "extra_promotion": null,
    "model_make_id": "Bmw",
    "model_name": "X3",
    "model_year": 2017,
    "model_trim": "",
    "model_seats": null,
    "model_doors": null,
    "images": [
        {
            "title": "Bmw X3 2017 White",
            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-9vodFLMxvn.JPG",
            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-QGGtSEFWl7.JPG"
        },
        {
            "title": "Bmw X3 2017 White",
            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-IYwEJlhImo.JPG",
            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-3yY6FetBLH.JPG",
            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-4HdU3XkFhJ.JPG"
        },
        {
            "title": "Bmw X3 2017 White",
            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-rA2AXGgMB3.JPG",
            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-JE0K3QpIwD.JPG",
            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-t0StfY0qUI.JPG"
        },
        {
            "title": "Bmw X3 2017 White",
            "url": "https://carhopper.co/uploads/2148/bmw-x3-2017-white-SQSF1gt16R.JPG",
            "thumb_url": "https://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-kvzHvFHMtQ.JPG",
            "mobile_url": "https://carhopper.co/uploads/2148/mobile/bmw-x3-2017-white-TnZ5BUJ5n7.JPG"
        }
    ],
    "location": {
        "id": 5,
        "name": "Los Angeles, CA"
    },
    "area": {
        "id": 10,
        "name": "Airport proximity"
    }
}
```

Gets the Car data.


### HTTP Request

`GET https://carhopper.co/api/v1/car/<ID>`


### URL Parameters

Parameter | Data Type | Description
--------- | ----------- | -----------
<ID> | int | Car ID
