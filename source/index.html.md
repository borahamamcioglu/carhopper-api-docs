---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell - cURL

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

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

CarHopper uses API keys to allow access to the API. You can get a CarHopper API key from your CarHopper profile page.

CarHopper expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6ImJhYmE1OTM3M2MzNDN`

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
		"id": 343,
		"avatar": "uploads\/1567\/thumb\/phpMqMQNC.jpg",
		"longitude": -80.1898443,
		"latitude": 25.8099336,
		"state_code": "",
		"city": "Miami",
		"state": "Florida",
		"country": "United States",
		"zip_code": "33137",
		"category": "Sedan",
		"gps": 0,
		"all_wheel_drive": 0,
		"audio_input": 1,
		"pet_friendly": 0,
		"sunroof": 1,
		"bike_rack": 0,
		"convertible": 0,
		"parking_aid": 1,
		"description": "Although we have Mercedes-Benz to thank for the oxymoronic \u201c4-door coupe\u201d category, Audi has gone one better with its A7 5-door coupe. Actually a 5-door liftback with a generous cargo hold and stunning silhouette, the 2014 Audi A7 incorporates everything right about Audi cars, from its stunning interior to its quattro all-wheel drive (AWD) to the myriad of engine choices and performance variants. Nestled nicely between the bread and butter A6 and the massive A8, the A7 holds special appeal to those who desire functionality and performance in an exclusive package that, while not appealing to everyone, is still attractive enough to catch everyone\u2019s eye.",
		"color": "White",
		"transmission_type": "Automatic",
		"delivery": 0,
		"delivery_price": "0.00",
		"hourly_price": "0.00",
		"daily_price": "360.00",
		"weekly_price": "2700.00",
		"monthly_price": "0.00",
		"extra_sunpass_price": "10.00",
		"extra_childseat_price": null,
		"extra_gps_price": null,
		"extra_delivery_price": "50.00",
		"insurance_cdw_price": null,
		"insurance_sli_price": null,
		"insurance_pai_price": null,
		"insurance_pec_price": null,
		"insurance_rcp_price": null,
		"deposit": 0,
		"milage_limit": null,
		"extra_milage": null,
		"extra_promotion": null,
		"model_make_id": "Audi",
		"model_name": "RS 7",
		"model_year": 2014,
		"model_seats": null,
		"model_doors": 4,
		"location": {
			"id": 1,
			"name": "Miami, FL"
		},
		"area": null
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
	"id": 350,
	"avatar": "uploads\/1567\/thumb\/phpMqMQNC.jpg",
	"longitude": -80.2608899,
	"latitude": 25.7999398,
	"state_code": "",
	"city": "Miami",
	"state": "Florida",
	"country": "United States",
	"zip_code": "33142",
	"category": "Sedan",
	"gps": 0,
	"all_wheel_drive": 0,
	"audio_input": 0,
	"pet_friendly": 0,
	"sunroof": 0,
	"bike_rack": 0,
	"convertible": 0,
	"parking_aid": 1,
	"description": "The 2016 Scion iM ushers in a new era of maturity for Toyota's youth-oriented brand.",
	"color": "Silver",
	"transmission_type": "Automatic",
	"delivery": 0,
	"delivery_price": "0.00",
	"hourly_price": "0.00",
	"daily_price": "100.00",
	"weekly_price": "180.00",
	"monthly_price": "0.00",
	"extra_sunpass_price": "2.99",
	"extra_childseat_price": null,
	"extra_gps_price": null,
	"extra_delivery_price": null,
	"insurance_cdw_price": "15.00",
	"insurance_sli_price": null,
	"insurance_pai_price": null,
	"insurance_pec_price": null,
	"insurance_rcp_price": null,
	"deposit": 0,
	"milage_limit": null,
	"extra_milage": null,
	"extra_promotion": null
}
```

### HTTP Request

`GET https://carhopper.co/api/v1/car/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the car to get details.