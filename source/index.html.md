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

Welcome to the CarHopper API! You can use our API to access CarHopper API endpoints, which can search and create reservations for luxury cars in our database.

We have language bindings in Shell. You can view code examples in the dark area to the right.

# Authentication

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer YOUR_API_KEY"
```


> Make sure to replace `YOUR_API_KEY` with your API key.

CarHopper uses API keys to allow access to the API. You can get a CarHopper API key from your CarHopper profile page. Be sure to use "Copy Link" in order to have it coppied the right way.

CarHopper expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer YOUR_API_KEY`

<aside class="warning">
You must replace <code>YOUR_API_KEY</code> with your personal API key.
</aside>

<aside class="warning">
Remember to add authentication key to every request!
</aside>

# Reservation

## Create

This endpoint creates reservations.

### HTTP Request

`GET https://carhopper.co/api/v1/create`


```shell
curl "https://carhopper.co/api/v1/create"
  -H "Authorization: Bearer YOUR_API_KEY"
```
> The above command returns JSON structured like this:

```json
{
	"status": 1,
	"message": "Reservation created",
	"data": {
		"reservation_id": 873
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
license_number | int | License number of the renter
license_state | string | Order results by `priceAsc, priceDesc, distanceAsc`
delivery_address | string | Delivery address


## Get

This endpoint is returning created reservation.
Only required field is <code>reservation_id</code>

```shell
curl "https://carhopper.co/api/v1/reservation/115"
-H "Authorization: Bearer YOUR_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
    "id": 42,
    "user_id": 24,
    "car_id": 84,
    "contact_email": null,
    "delivery": 0,
    "delivery_price": "0.00",
    "price": "3.00",
    "days": 1,
    "service_fee": "0.75",
    "what_you_pocket": null,
    "carhopper_fee": null,
    "age_19_20_fee": null,
    "age_21_25_fee": null,
    "suv_fee": null,
    "car_age_fee": null,
    "discount": "0.00",
    "total_price": "3.75",
    "insurance": null,
    "extras": null,
    "coupon_id": null,
    "start_at": "2016-07-24 04:00:00",
    "end_at": "2016-07-26 04:00:00",
    "created_at": "2015-09-29 11:09:24",
    "responded_at": "2015-09-29 12:27:09",
    "deleted_at": null,
    "status": "paid",
    "referral_discount": "0.00",
    "auto_decline": 1,
    "delivery_address": null,
    "company_commission": null,
    "notes": null,
    "coupon": null
}
```

### HTTP Request

`GET https://carhopper.co/api/v1/reservation/<ID>`



## Cancel

This endpoint cancels the previously created reservation.
Only required field is <code>reservation_id</code>

```shell
curl "https://carhopper.co/api/v1/cancel/5"
  -H "Authorization: Bearer YOUR_API_KEY"
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

### HTTP Request

`GET https://carhopper.co/api/v1/cancel/<ID>`


## Complete

This endpoint completes the reservation process.
Only required field is <code>reservation_id</code>


```shell
curl "https://carhopper.co/api/v1/complete/5"
  -H "Authorization: Bearer YOUR_API_KEY"
```

### HTTP Request

`GET https://carhopper.co/api/v1/complete/<ID>`


# Search

## Search Cars


```shell
curl "https://carhopper.co/api/v1/search"
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
        "avatar": "http://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
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
        "delivery_price": "0.00",
        "hourly_price": "0.00",
        "daily_price": "150.00",
        "weekly_price": "900.00",
        "monthly_price": "0.00",
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
                "url": "uploads/2148/bmw-x3-2017-white-9vodFLMxvn.JPG",
                "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
                "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-QGGtSEFWl7.JPG"
            },
            {
                "title": "Bmw X3 2017 White",
                "url": "uploads/2148/bmw-x3-2017-white-IYwEJlhImo.JPG",
                "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-3yY6FetBLH.JPG",
                "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-4HdU3XkFhJ.JPG"
            },
            {
                "title": "Bmw X3 2017 White",
                "url": "uploads/2148/bmw-x3-2017-white-rA2AXGgMB3.JPG",
                "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-JE0K3QpIwD.JPG",
                "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-t0StfY0qUI.JPG"
            },
            {
                "title": "Bmw X3 2017 White",
                "url": "uploads/2148/bmw-x3-2017-white-SQSF1gt16R.JPG",
                "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-kvzHvFHMtQ.JPG",
                "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-TnZ5BUJ5n7.JPG"
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

`GET https://carhopper.co/api/v1/search`

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

<aside class="success">
You can use location <code>id</code> to filter cars or get related areas.
</aside>

### HTTP Request

`GET https://carhopper.co/api/v1/locations`

### URL Parameters

Parameter | Description
--------- | -----------
No parameters required | - 

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

This endpoint retrieves areas related to given <code>location_id</code>

<aside class="success">
You can use area <code>id</code> to filter cars.
</aside>

### HTTP Request

`GET https://carhopper.co/api/v1/areas`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the location that areas related to.

## Car Details

```shell
curl "https://carhopper.co/api/v1/car/350"
  -H "Authorization: Bearer YOUR_API_KEY"
```

> The above command returns JSON structrued like this:

```json
{
    "id": 1379,
    "avatar": "http://carhopper.co/uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
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
    "delivery_price": "0.00",
    "hourly_price": "0.00",
    "daily_price": "150.00",
    "weekly_price": "900.00",
    "monthly_price": "0.00",
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
            "url": "uploads/2148/bmw-x3-2017-white-9vodFLMxvn.JPG",
            "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-cNDRfHLyok.JPG",
            "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-QGGtSEFWl7.JPG"
        },
        {
            "title": "Bmw X3 2017 White",
            "url": "uploads/2148/bmw-x3-2017-white-IYwEJlhImo.JPG",
            "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-3yY6FetBLH.JPG",
            "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-4HdU3XkFhJ.JPG"
        },
        {
            "title": "Bmw X3 2017 White",
            "url": "uploads/2148/bmw-x3-2017-white-rA2AXGgMB3.JPG",
            "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-JE0K3QpIwD.JPG",
            "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-t0StfY0qUI.JPG"
        },
        {
            "title": "Bmw X3 2017 White",
            "url": "uploads/2148/bmw-x3-2017-white-SQSF1gt16R.JPG",
            "thumb_url": "uploads/2148/thumb/bmw-x3-2017-white-kvzHvFHMtQ.JPG",
            "mobile_url": "uploads/2148/mobile/bmw-x3-2017-white-TnZ5BUJ5n7.JPG"
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

### HTTP Request

`GET https://carhopper.co/api/v1/car/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the car to get details.
