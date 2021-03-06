---
title: Distribusion API Documentation

language_tabs:
  - shell: cURL

toc_footers:
  - <a href='mailto:partner@distribusion.com'>Sign Up for a Demo API Key</a>
  - <a href='http://distribusion.com/'>Visit our website</a>
  - <a href='mailto:partner@distribusion.com'>Contact Us</a>

includes:
  - errors
  - dictionary

search: true
---

# Introduction to Distribusion API

Dear partner,

You made it - in front of you lies the most loved API in transporation industry. Actually it is mostly the love of our dev and product team so far due to it being super brand new, but hope you will fall in love soon too. Anytime feel free to write to me personally to [partner@distribusion.com](mailto:partner@distribusion.com) in case you are unhappy or our baby is not behaving as you expected.

You can trust on us working hard everyday to make it your most loved product.

Your Distribusion IT team

![alt text](/images/distribusion-it-team.png)

You can use our API to access Distribusion content and book bus rides. We have put a lot of thoughts into making it as easy as possible to integrate with us. Have fun and share your feedback with us!

In this new API, Distribusion aim to standardize the stations codes across the bus industry, the same way IATA did in the flight industry. Our stations code are designed to represent specific locations using a set of 8 capital letters. The construction of such code follow a simple logic:

- The first two letters represent the country
- The next three, the city in which the station is located
- The last three letters represent a more precise location

Example: GBLONVSA - Great Britain / London / Victoria station

When integrating and testing our API, please use the following URL: `https://api-demo.distribusion.com/retailers/v3`
Documentation for the API v2 can still be found at [https://api-demo.distribusion.com/reseller/v2/docs](https://api-demo.distribusion.com/reseller/v2/docs)

# Getting Started

When performing a booking through Distribusion API, they are multiple steps that are required. This section is dedicated to describe the standard flow, necessary to search for connections, retrieve availability and prices, build the booking form and perform a booking.

![alt text](/images/API_V3.0_workflow.png)  

## Collect Initial Data

The first step in the booking process, is the collection of the stations and marketing carriers available through our API. To do so, we provide a [#stations](https://api-demo.distribusion.com/retailers/v3/docs/#stations) and a [#marketing_carriers](https://api-demo.distribusion.com/retailers/v3/docs/#marketing-carriers) endpoints. To retrieve stations codes, simply send a GET request towards our [#stations](https://api-demo.distribusion.com/retailers/v3/docs/#stations) endpoint, which will send you a list of the stations, with stations codes and additional information concerning the stations (address, GPS location, associated city, etc). The [#marketing_carriers](https://api-demo.distribusion.com/retailers/v3/docs/#marketing-carriers) endpoint, provides you with additional information related to a specific marketing carrier; such as logos, contact details, cancellation policy as well as terms and conditions.

## Find Connections

After retrieving station codes, you can now start searching for connections by sending us `GET` requests towards [connections#find](https://api-demo.distribusion.com/retailers/v3/docs/#find). We do not provide stations pair in order to allow our engines to learn from the consumers demand. By doing so, if a specific ride is highly requested but currently unavailable, you can be assured that it will be soon delivered through our API.

<aside class="success">
Remember — to query information from our API, you need to get your own API key!
</aside>

## Build a Booking Form

In order to accomodate the specific requirements of each marketing carrier and increase conversion rate, we also deliver a dynamic booking form. Mandatory information as well as the validation rules applied to each parameters are provided through our [bookings#form_schema](https://api-demo.distribusion.com/retailers/v3/docs/#form-schema) endpoint.

## Confirm Price and Availability

As [connections#find](https://api-demo.distribusion.com/retailers/v3/docs/#find) provides the cheapest adult price, it is some time necessary to confirm the price of specific passenger types by calling [connections#vacancy](https://api-demo.distribusion.com/retailers/v3/docs/#vacancy). This call is also recommended prior to perform a booking, in order to ensure the prices and availability are still up to date.

## Create a Booking

After gathering information on a specific connection and the passenger information, you can now send a booking request towards Distribusion's API on [bookings#create](https://api-demo.distribusion.com/retailers/v3/docs/#create). The parameters to be sent are dependant of the marketing carrier and should have been delivered to you through the [bookings#form_schema](https://api-demo.distribusion.com/retailers/v3/docs/#form-schema). In case any information is missing, the API will recognise it and inform you accordingly.

## Postman

Here’s a collection of sample queries in Postman that’ll help you get up to speed with our API faster.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/f43f2b32817c094f82b0)

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Api-Key: AIzaSyBGEpZdxbufTSpcIxWXoRjSdKu6ZctiuyI"
```

> Make sure to replace `AIzaSyBGEpZdxbufTSpcIxWXoRjSdKu6ZctiuyI` with your API key.

Distribusion uses API keys to allow access to the API content and perform bookings. You can register a new Distribusion API key by contacting us at [partner@distribusion.com](mailto:partner@distribusion.com).

Distribusion expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Api-Key:AIzaSyBGEpZdxbufTSpcIxWXoRjSdKu6ZctiuyI`

<aside class="notice">
You must replace <code>AIzaSyBGEpZdxbufTSpcIxWXoRjSdKu6ZctiuyI</code> with your own API key.
</aside>

# Connections

## Find

```shell
curl 'https://api.distribusion.com/retailers/v3/connections/find?departure_stations%5B%5D=DEBERZOB&arrival_stations%5B%5D=DEBONHBF&departure_date=2017-01-01&pax=1&locale=en&currency=EUR' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "currency": "EUR",
    "locale": "en"
  },
  "data": [
    {
      "id": "JETB-DEDUSZOB-DEWEZFLU-2017-04-29T14:00-2017-04-29T15:30",
      "type": "connections",
      "attributes": {
        "departure_time": "2017-04-29T14:00",
        "arrival_time": "2017-04-29T15:30",
        "duration": 5400,
        "cheapest_total_adult_price": 1500,
        "booked_out": false
      },
      "relationships": {
        "departure_station": {
          "data": {
            "id": "DEDUSZOB",
            "type": "stations"
          }
        },
        "arrival_station": {
          "data": {
            "id": "DEWEZFLU",
            "type": "stations"
          }
        },
        "marketing_carrier": {
          "data": {
            "id": "JETB",
            "type": "marketing_carriers"
          }
        }
      }
    }
  ],
  "included": [
    {
      "id": "DEDUSZOB",
      "type": "stations",
      "attributes": {
        "code": "DEDUSZOB",
        "name": "Düsseldorf ZOB",
        "description": "The stop is located at the north end of the central railway station, directly next to the municipal public order office (\"Ordnungsamt\"). At the stop, there is a small snack bar as well as a covered waiting cabin.",
        "street_and_number": "Worringer Straße",
        "zip_code": "40210",
        "longitude": 6.79472580000004,
        "latitude": 51.2223151,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEDUS",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "DEWEZFLU",
      "type": "stations",
      "attributes": {
        "code": "DEWEZFLU",
        "name": "Weeze Flughafen",
        "description": "The bus stop is located directly in front of the terminal entrance.",
        "street_and_number": "Flughafen-Ring 60",
        "zip_code": "47652",
        "longitude": 6.16098999999997,
        "latitude": 51.59388,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEWEZ",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "JETB",
      "type": "marketing_carriers",
      "attributes": {
        "code": "JETB",
        "trade_name": "Jetbus",
        "legal_name": "RheinJet GmbH",
        "booking_fee": 0
      },
      "relationships": {
        "passenger_types": {
          "data": [
            {
              "id": "PNOR",
              "type": "passenger_types"
            }
          ]
        }
      }
    },
    {
      "id": "PNOR",
      "type": "passenger_types",
      "attributes": {
        "code": "PNOR",
        "name": "Normal",
        "description": null
      }
    },
    {
      "id": "DEDUS",
      "type": "cities",
      "attributes": {
        "name": "Dusseldorf"
      }
    },
    {
      "id": "DEWEZ",
      "type": "cities",
      "attributes": {
        "name": "Weeze"
      }
    }
  ]
}
```

The endpoint searches for and delivers all the connections matching your query. The delivered set of objects will have all the necessary data for displaying on the search results page.

On our side we parallelize the outbound requests to the operating carrier integration systems to include as many connections as we're able to get from marketing carriers (excluding those which couldn't respond in time).


### HTTP Request

`GET https://api.distribusion.com/retailers/v3/connections/find`

### Query Parameters

Parameter               | Mandatory | Description
----------------------- | --------- | -----------
`departure_stations[]`  | true      | 8- or 9-letter alphanumeric uppercase code, available codes can be obtained from `GET /stations`.
`arrival_stations[]`    | true      | 8- or 9-letter alphanumeric uppercase code, available codes can be obtained from `GET /stations`.
`departure_date`        | true      | Departure date in ISO 8601 format.
`pax`                   | true      | Number of passengers - Integer in a range of 1..9.
`locale`                | true      | 2-letter alphanumeric lowercase code, according to ISO 639-1 standard .
`currency`              | true      | 3-letter alphanumeric uppercase code, according to ISO 4217 standard.

<aside class="success">
Remember — to query information from our API, you need to get your own API key at http://distribusion.com/!
</aside>

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.101.002 | Departure stations are invalid or missing
400.101.003 | Arrival stations are invalid or missing
400.101.014 | Locale is invalid or missing
400.101.015 | Currency is invalid or missing
400.200.001 | API key is invalid or missing
400.501.000 | Not implemented at Distribusion on connections#find
400.800.000 | Not Found
500.100.000 | Service Unavailable
600.000.000 | Marketing Carrier remote system unavailable
600.100.000 | Marketing Carrier remote system time-out
600.400.000 | Marketing Carrier remote request invalid
600.900.000 | Response received from a remote server technically cannot be processed
600.901.000 | Response from remote server logically cannot be processed (is invalid)

## Vacancy

```shell
curl 'https://api.distribusion.com/retailers/v3/connections/vacancy?marketing_carrier=SPBS&departure_station=DEBERZOB&arrival_station=DEBONHBF&departure_time=2017-01-01T00:00&arrival_time=2017-01-01T01:00&pax=1&locale=en&currency=EUR' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "currency": "EUR"
  },
  "data": {
    "id": "JETB-DEDUSZOB-DEWEZFLU-2017-04-29T14:00-2017-04-29T15:30",
    "type": "connections",
    "attributes": {
      "vacant": true,
      "total_price": 1500
    }
  }
}
```

This endpoint should be used during a booking to get a binding price and availability for a specified group of travellers. "Specified" is defined as having not only the right amount of passengers but also selected the passenger types for each passenger. The passenger types are marketing carrier specific (retrieved from connections#find or using #marketing_carriers). If you are looking into getting the general availability of a connection (true / false) please use the connections#find endpoint, which retrieve the availability directly from marketing carriers.

### HTTP Request

`GET api.distribusion.com/retailers/v3/connections/vacancy`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`marketing_carrier` | true      | 4-letter alphanumeric uppercase code.
`departure_station` | true      | 8 or 9-letter alphanumeric uppercase code.
`arrival_station`   | true      | 8 or 9-letter alphanumeric uppercase code.
`departure_time`    | true      | Departure time in ISO 8601 format without timezone yyyy-mm-ddThh:mm.
`arrival_time`      | true      | Arrival time in ISO 8601 format without timezone yyyy-mm-ddThh:mm.
`pax`               | true      | Number of passengers - Integer in a range of 1..9.
`locale`            | true      | 2-letter alphanumeric lowercase code, according to ISO 639-1 standard.
`currency`          | true      | 3-letter alphanumeric uppercase code, according to ISO 4217 standard.

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.101.004 | Departure station is invalid or missing
400.101.005 | Arrival station is invalid or missing
400.101.009 | PAX is invalid or missing
400.101.014 | Locale is invalid or missing
400.101.015 | Currency is invalid or missing
400.101.044 | Departure date is in the past
400.101.049 | Departure time must be before arrival time
400.102.040 | Unknown marketing carrier
400.200.001 | API key is invalid or missing
400.501.002 | Not implemented at Distribusion on connections#vacancy
400.800.000 | Not Found
400.800.040 | Marketing carrier not found
500.100.000 | Service Unavailable
600.000.000 | Marketing Carrier remote system unavailable
600.100.000 | Marketing Carrier remote system time-out
600.400.000 | Marketing Carrier remote request invalid
600.500.000 | Marketing Carrier remote connection unknown error
600.600.000 | Marketing Carrier remote vacancy unknown error
600.900.000 | Response received from a remote server technically cannot be processed
600.901.000 | Response from remote server logically cannot be processed (is invalid)
600.703.026 | Booking price unknown

# Bookings

## Form Schema

```shell
curl 'https://api.distribusion.com/retailers/v3/bookings/form_schema?marketing_carrier=SPBS' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "marketing_carrier": {
      "type": "string",
      "pattern": "^[A-Z]{4}$"
    },
    "departure_station": {
      "type": "string",
      "pattern": "^[A-Z]{8,9}$"
    },
    "arrival_station": {
      "type": "string",
      "pattern": "^[A-Z]{8,9}$"
    },
    "departure_time": {
      "type": "string",
      "pattern": "^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}"
    },
    "arrival_time": {
      "type": "string",
      "pattern": "^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}"
    },
    "retailer_partner_number": {
      "type": "string",
      "pattern": "^[0-9]{6}$"
    },
    "title": {
      "type": "string",
      "enum": [
        "mr",
        "mrs"
      ]
    },
    "first_name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 50
    },
    "last_name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 50
    },
    "street_and_number": {
      "type": "string"
    },
    "zip_code": {
      "type": "string"
    },
    "city": {
      "type": "string"
    },
    "email": {
      "type": "string",
      "format": "email"
    },
    "phone": {
      "type": "string"
    },
    "execute_payment": {
      "type": "boolean"
    },
    "payment_method": {
      "type": "string",
      "enum": [
        "demand_note",
        "credit_card",
        "sepa_direct_debit",
        "paypal"
      ]
    },
    "payment_token": {
      "type": "string"
    },
    "payer_id": {
      "type": "string"
    },
    "total_price": {
      "type": "integer"
    },
    "pax": {
      "type": "integer"
    },
    "flight_number": {
      "type": "string",
      "pattern": "^[0-9A-Z]{3,9}$"
    },
    "terms_accepted": {
      "type": "boolean"
    },
    "locale": {
      "type": "string",
      "pattern": "^[a-z]{2}$"
    },
    "currency": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "send_customer_email": {
      "type": "boolean"
    },
    "passengers": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "first_name": {
            "type": "string"
          },
          "last_name": {
            "type": "string"
          },
          "type": {
            "type": "string",
            "pattern": "^[A-Z]{4}$"
          }
        },
        "additionalProperties": false,
        "required": [
          "first_name",
          "last_name",
          "type"
        ]
      }
    },
    "extras": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "quantity": {
            "type": "integer"
          },
          "type": {
            "type": "string"
          }
        },
        "additionalProperties": false,
        "required": [
          "quantity",
          "code",
          "type"
        ]
      }
    }
  },
  "additionalProperties": false,
  "required": [
    "marketing_carrier",
    "departure_station",
    "arrival_station",
    "departure_time",
    "arrival_time",
    "retailer_partner_number",
    "title",
    "first_name",
    "last_name",
    "street_and_number",
    "zip_code",
    "city",
    "email",
    "execute_payment",
    "payment_method",
    "total_price",
    "pax",
    "terms_accepted",
    "locale",
    "currency",
    "send_customer_email"
  ]
}
```

To create a booking, you will need to retrieve specific information from the customer. In order to do so, you can render a booking form, which is specific to a certain connection and or marketing carrier. This endpoint will deliver you the JSON schema, describing the necessary parameters needed to perform a booking.

### HTTP Request

`GET api.distribusion.com/retailers/v3/bookings/form_schema`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`marketing_carrier` | true      | 4-letter alphanumeric uppercase code.

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.101.040 | Marketing carrier is invalid or missing
400.501.004 | Not implemented at Distribusion on bookings#form_schema
400.800.000 | Not Found
400.800.040 | Marketing carrier not found
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable


## Create

```shell
curl -i --data '{"marketing_carrier": "SPBS","departure_station": "DEBERZOB","arrival_station": "DEBONHBF","departure_time": "2017-01-01T00:00","arrival_time": "2017-01-01T01:00","title": "mr","first_name": "John","last_name": "Doe","email": "john.doe@customer.example","phone": "+0 123 456 78 90","city": "Berlin","zip_code": "10123","street_and_number": "1st str.","execute_payment": true,"payment_method": "demand_note","payment_token": "da0b3dfb-9cdf-4c17-8549-e179bad45765","payer_id": "7E7MGXCWTTKK2","total_price": "1100","pax": 1,"flight_number": "141DE","terms_accepted": true,"locale": "en","currency": "EUR","send_customer_email": true,"passengers": [{"first_name": "Janeth","last_name": "Doe","type": "ADLT"}],"extras": [{"quantity": 1,"type": "COFE"}]}' 'https://api.distribusion.com/retailers/v3/bookings/create' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "locale": "en",
    "currency": "EUR"
  },
  "data": {
    "id": "ij8CE-9Wo-cQvrj7D5gQzA",
    "type": "bookings",
    "attributes": {
      "departure_time": "2017-04-29T14:00",
      "arrival_time": "2017-04-29T15:30",
      "duration": 5400,
      "title": "mr",
      "first_name": "Balibalo",
      "last_name": "Bateau",
      "email": "test@mail.com",
      "phone": "4915237601929",
      "city": "Berlin",
      "zip_code": "10123",
      "street_and_number": "Berlinstr. 23",
      "execute_payment": false,
      "payment_method": "demand_note",
      "payment_token": null,
      "payer_id": "123455",
      "total_price": 1500,
      "pax": 1,
      "flight_number": null,
      "distribusion_booking_number": "N3TNJK",
      "marketing_carrier_booking_number": "JWspC1GQf8vV12AZjA4NFA",
      "terms_accepted": true,
      "send_customer_email": true,
      "retailer_partner_number": "222222",
      "created_at": "2017-04-21T18:35"
    },
    "relationships": {
      "departure_station": {
        "data": {
          "id": "DEDUSZOB",
          "type": "stations"
        }
      },
      "arrival_station": {
        "data": {
          "id": "DEWEZFLU",
          "type": "stations"
        }
      },
      "marketing_carrier": {
        "data": {
          "id": "JETB",
          "type": "marketing_carriers"
        }
      },
      "operating_carrier": {
        "data": {
          "id": "JETB",
          "type": "operating_carriers"
        }
      },
      "passengers": {
        "data": [
          {
            "id": "PNOR-BATEAU-BALIBALO",
            "type": "passengers"
          }
        ]
      },
      "extras": {
        "data": null
      }
    }
  },
  "included": [
    {
      "id": "DEDUSZOB",
      "type": "stations",
      "attributes": {
        "code": "DEDUSZOB",
        "name": "Düsseldorf ZOB",
        "description": "The stop is located at the north end of the central railway station, directly next to the municipal public order office (\"Ordnungsamt\"). At the stop, there is a small snack bar as well as a covered waiting cabin.",
        "street_and_number": "Worringer Straße",
        "longitude": 6.79472580000004,
        "latitude": 51.2223151,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEDUS",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "DEWEZFLU",
      "type": "stations",
      "attributes": {
        "code": "DEWEZFLU",
        "name": "Weeze Flughafen",
        "description": "The bus stop is located directly in front of the terminal entrance.",
        "street_and_number": "Flughafen-Ring 60",
        "longitude": 6.16098999999997,
        "latitude": 51.59388,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEWEZ",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "JETB",
      "type": "marketing_carriers",
      "attributes": {
        "code": "JETB",
        "trade_name": "Jetbus",
        "legal_name": "RheinJet GmbH",
        "address": "Am Hövel 9, 40667 Meerbusch",
        "phone": "089 230 505 05",
        "fax": null,
        "customer_service_phone": "+49 89 230 505 05",
        "email": "service@rheinjet.de",
        "commercial_register": "Amtsgericht Neuss",
        "commercial_register_number": "HRB 13389",
        "authorised_representative": "Martin Michael",
        "white_label_logo": "data:image/png;base64, picture",
        "white_label_colour_code": null,
        "terms": "T&C",
        "flight_number_required": true,
        "booking_fee": 0,
        "cancellation_fee": 1500,
        "cancellation_cutoff": 720
      }
    },
    {
      "id": "JETB",
      "type": "operating_carriers",
      "attributes": {
        "code": "JETB",
        "trade_name": "Jetbus",
        "legal_name": "RheinJet GmbH"
      }
    },
    {
      "id": "PNOR-BATEAU-BALIBALO",
      "type": "passengers",
      "attributes": {
        "first_name": "Balibalo",
        "last_name": "Bateau",
        "type": "PNOR",
        "price": 1500
      }
    },
    {
      "id": "DEDUS",
      "type": "cities",
      "attributes": {
        "name": "Dusseldorf"
      }
    },
    {
      "id": "DEWEZ",
      "type": "cities",
      "attributes": {
        "name": "Weeze"
      }
    }
  ]
}
```

Use this endpoint to conduct bookings. Bookings created this way are the basis for invoicing. The endpoint currently supports one-way bookings only. It will also send out booking e-mails to customers. E-Mails are blocked on demo, so feel free to conduct tests bookings on our demo server (api-demo.distribusion.com).


### HTTP Request

`POST https://api.distribusion.com/retailers/v3/bookings`

### Query Parameters

Parameter                 | Mandatory | Description
------------------------- | :-------: | :----------
`marketing_carrier`       | true      | 4-letter alphanumeric uppercase code.
`departure_station`       | true      | 8- or 9-letter alphanumeric uppercase code.
`arrival_station`         | true      | 8- or 9-letter alphanumeric uppercase code.
`departure_time`          | true      | Departure time in ISO 8601 format without timezone yyyy-mm-ddThh:mm.
`arrival_time`            | true      | departure time in ISO 8601 format without timezone yyyy-mm-ddThh:mm.
`retailer_partner_number` | true      | 6 digits number of retailer partner.
                          |           |
`title`                   | true      | Allowed values: "mr" or "mrs".
`first_name`              | true      | First name of the customer, 1-50-letter alphanumeric string.
`last_name`               | true      | Last name of the customer, 1-50-letter alphanumeric string.
`email`                   | true      | Email address of the customer.
`phone`                   | true      | Phone number of the customer, 8-20-number string.
`city`                    | true      | 1-50-letter alphanumeric string.
`zip_code` | true | 2-10-letter alphanumeric string.
`street_and_number`       | true      | 1-99-letter alphanumeric string.
                          |           |
`execute_payment`         | true      | Can be `true` or `false`. Flag signalling whether the payment transaction should be executed.
`payment_method`          | true      | Allowed value: `demand_note`, `credit_card`, `sepa_direct_debit`, `paypal`.
`payment_token`           | false     | Unique identifier supplied by the payment service provider.
`payer_id`                | false     | Unique identifier supplied by Paypal.
`total_price`             | true      | Total price of the requested passenger types and extras with added bus marketing carrier booking fee in fractional currency unit (if there is one, in cent) in requested currency. Minimum value - 0, maximum value - 9999900. Provided on successful vacancy request.
`pax`                     | true      | Number of passengers. Integer in a range of 1..9.
                          |           |
 `flight_number`          | false     | Required by marketing carriers in some cases. 3-9-letter alphanumeric uppercase code.
 `terms_accepted`         | true      | Flag signalling whether Terms & Conditions of the marketing carrier have been accepted. Can be `true` or `false`.
                          |           |  
`locale`                  | true      | 2-letter alphanumeric lowercase code, according to ISO 639-1 standard.
`currency`                | true      | 3-letter alphanumeric uppercase code, according to ISO 4217 standard.
`send_customer_email`     | true      | Flag for sending out (or preventing) of the customer confirmation email from Distribusion. Can be `true` or `false`.
                          |           |
`passengers`              | true      | Array
`fist_name`               | true      | First name of the passenger, 1-50-letter alphanumeric string.
`last_name`               | true      | Last name of the customer, 1-50-letter alphanumeric string.
`type`                    | true      | Passenger type. 4-letter alphanumeric uppercase code, available codes can be obtained from `GET /marketing_carriers`.
                          |           |
`extras`                  | false     | Array
`quantity`                | true      | Integer in a range of 1..999.
`type`                    | true      | Extra type. 4-letter alphanumeric uppercase code, available codes can be obtained from `GET /marketing_carriers`.


<aside class="warning">When sending request to our production environment, ensure that you don't play around with creating bookings, as you will have to pay for them! </aside>

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.101.004 | Departure station is invalid or missing
400.101.005 | Arrival station is invalid or missing
400.101.007 | Departure time is invalid or missing
400.101.008 | Arrival time is invalid or missing
400.101.009 | PAX is invalid or missing
400.101.010 | Retailer partner number is invalid or missing
400.101.011 | Payment method is invalid or missing
400.101.012 | Payment token is invalid or missing
400.101.013 | Payer ID is invalid or missing
400.101.014 | Locale is invalid or missing
400.101.015 | Currency is invalid or missing
400.101.018 | Title is invalid or missing
400.101.019 | First name is invalid or missing
400.101.020 | Last name is invalid or missing
400.101.021 | Email is invalid or missing
400.101.022 | Phone is invalid or missing
400.101.023 | City is invalid or missing
400.101.024 | Zip code is invalid or missing
400.101.025 | Street and number is invalid or missing
400.101.026 | Total price is invalid or missing
400.101.027 | Customer currency is invalid or missing
400.101.029 | Send customer email is invalid or missing
400.101.030 | Passenger first name is invalid or missing
400.101.031 | Passenger last name is invalid or missing
400.101.032 | Passenger type is invalid or missing
400.101.033 | Passenger type pax is invalid or missing
400.101.034 | Passenger type code is missing or invalid
400.101.035 | Terms accepted is invalid or missing
400.101.036 | Flight number is invalid or missing
400.101.037 | Extra type is invalid or missing
400.101.038 | Extra quantity is invalid or missing
400.101.040 | Marketing carrier is invalid or missing
400.101.042 | Execute payment is invalid or missing
400.101.044 | Departure date is in the past
400.101.049 | Departure time must be before arrival time
400.101.050 | Extras are invalid or missing
400.101.051 | Passengers are invalid or missing
400.102.040 | Unknown marketing carrier
400.200.001 | API key is invalid or missing
400.501.003 | Not implemented at Distribusion on bookings#create
400.800.000 | Not Found
400.800.040 | Marketing carrier not found
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable
500.500.017 | Unique distribusion booking number cannot be generated
500.500.041 | Unique booking id cannot be generated
500.501.000 | Booking not vacant
500.502.026 | Booking price increased
500.502.026 | Booking price decreased
600.000.000 | Marketing Carrier remote system unavailable
600.100.000 | Marketing Carrier remote system time-out
600.400.000 | Marketing Carrier remote request invalid
600.500.000 | Marketing Carrier remote connection unknown error
600.600.000 | Marketing Carrier remote vacancy unknown error
600.700.000 | Marketing Carrier remote booking failed error (general)
600.701.000 | Marketing Carrier remote booking capacity error
600.702.000 | Marketing Carrier remote booking price change error
600.703.000 | Marketing Carrier remote booking tariff unavailable error
600.705.000 | Marketing Carrier remote booking vacancy unknown error
600.900.000 | Response received from a remote server technically cannot be processed
600.901.000 | Response from remote server logically cannot be processed (is invalid)
600.703.026 | Booking price unknown


## Show

```shell
curl 'https://api.distribusion.com/retailers/v3/bookings/show?booking=ij8CE-9Wo-cQvrj7D5gQzA' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "locale": "en",
    "currency": "EUR"
  },
  "data": {
    "id": "ij8CE-9Wo-cQvrj7D5gQzA",
    "type": "bookings",
    "attributes": {
      "departure_time": "2017-04-29T14:00",
      "arrival_time": "2017-04-29T15:30",
      "duration": 5400,
      "title": "mr",
      "first_name": "Balibalo",
      "last_name": "Bateau",
      "email": "test@mail.com",
      "phone": "4915237601929",
      "city": "Berlin",
      "zip_code": "10123",
      "street_and_number": "Berlinstr. 23",
      "execute_payment": false,
      "payment_method": "demand_note",
      "payment_token": null,
      "payer_id": "123455",
      "total_price": 1500,
      "pax": 1,
      "flight_number": null,
      "distribusion_booking_number": "N3TNJK",
      "marketing_carrier_booking_number": "JWspC1GQf8vV12AZjA4NFA",
      "terms_accepted": true,
      "send_customer_email": true,
      "retailer_partner_number": "222222",
      "created_at": "2017-04-21T18:35"
    },
    "relationships": {
      "departure_station": {
        "data": {
          "id": "DEDUSZOB",
          "type": "stations"
        }
      },
      "arrival_station": {
        "data": {
          "id": "DEWEZFLU",
          "type": "stations"
        }
      },
      "marketing_carrier": {
        "data": {
          "id": "JETB",
          "type": "marketing_carriers"
        }
      },
      "operating_carrier": {
        "data": {
          "id": "JETB",
          "type": "operating_carriers"
        }
      },
      "passengers": {
        "data": [
          {
            "id": "PNOR-BATEAU-BALIBALO",
            "type": "passengers"
          }
        ]
      },
      "extras": {
        "data": null
      }
    }
  },
  "included": [
    {
      "id": "DEDUSZOB",
      "type": "stations",
      "attributes": {
        "code": "DEDUSZOB",
        "name": "Düsseldorf ZOB",
        "description": "The stop is located at the north end of the central railway station, directly next to the municipal public order office (\"Ordnungsamt\"). At the stop, there is a small snack bar as well as a covered waiting cabin.",
        "street_and_number": "Worringer Straße",
        "longitude": 6.79472580000004,
        "latitude": 51.2223151,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEDUS",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "DEWEZFLU",
      "type": "stations",
      "attributes": {
        "code": "DEWEZFLU",
        "name": "Weeze Flughafen",
        "description": "The bus stop is located directly in front of the terminal entrance.",
        "street_and_number": "Flughafen-Ring 60",
        "longitude": 6.16098999999997,
        "latitude": 51.59388,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEWEZ",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "JETB",
      "type": "marketing_carriers",
      "attributes": {
        "code": "JETB",
        "trade_name": "Jetbus",
        "legal_name": "RheinJet GmbH",
        "address": "Am Hövel 9, 40667 Meerbusch",
        "phone": "089 230 505 05",
        "fax": null,
        "customer_service_phone": "+49 89 230 505 05",
        "email": "service@rheinjet.de",
        "commercial_register": "Amtsgericht Neuss",
        "commercial_register_number": "HRB 13389",
        "authorised_representative": "Martin Michael",
        "white_label_logo": "data:image/png;base64, picture",
        "white_label_colour_code": null,
        "terms": "T&C",
        "flight_number_required": true,
        "booking_fee": 0,
        "cancellation_fee": 1500,
        "cancellation_cutoff": 720
      }
    },
    {
      "id": "JETB",
      "type": "operating_carriers",
      "attributes": {
        "code": "JETB",
        "trade_name": "Jetbus",
        "legal_name": "RheinJet GmbH"
      }
    },
    {
      "id": "PNOR-BATEAU-BALIBALO",
      "type": "passengers",
      "attributes": {
        "first_name": "Balibalo",
        "last_name": "Bateau",
        "type": "PNOR",
        "price": 1500
      }
    },
    {
      "id": "DEDUS",
      "type": "cities",
      "attributes": {
        "name": "Dusseldorf"
      }
    },
    {
      "id": "DEWEZ",
      "type": "cities",
      "attributes": {
        "name": "Weeze"
      }
    }
  ]
}
```

To review data of a booking at a later stage without copying it into your system, you can use our bookings#show endpoint to retrieve it and show its data.

### HTTP Request

`GET api.distribusion.com/retailers/v3/bookings/show`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`booking`           | true      | 22-letter alphanumeric lowercase code, provided on successful booking.

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.100.041 | Booking is invalid or missing
400.200.001 | API key is invalid or missing
400.800.000 | Not Found
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable
400.501.005 | Not implemented at Distribusion on bookings#show
400.800.041 | Booking not found

## Tickets

```shell
curl 'https://api.distribusion.com/retailers/v3/bookings/ij8CE-9Wo-cQvrj7D5gQzA/tickets' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

This endpoint is dedicated to retailers sending their own confirmation email. If you have set `send_customer_email` to `true`, you do not need this endpoint. The ticket either has one page containing the information of all the passengers, or it consists of one page per passenger (in PDF format).

### HTTP Request

`GET api.distribusion.com/retailers/v3/bookings/{id}/tickets`

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.100.041 | Booking is invalid or missing
400.200.001 | API key is invalid or missing
400.800.000 | Not Found
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable
400.501.006 | Not implemented at Distribusion on bookings#ticket
400.800.041 | Booking not found

# Cancellations

## Conditions

```shell
curl 'https://api.distribusion.com/retailers/v3/cancellations/conditions?booking=ij8CE-9Wo-cQvrj7D5gQzA' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "currency": "EUR"
  },
  "data": {
    "id": "ij8CE-9Wo-cQvrj7D5gQzA",
    "type": "cancellations",
    "attributes": {
      "allowed": true,
      "fee": 1500,
      "cutoff": "2017-04-21T18:23:27.000+02:00"
    }
  }
}
```

This endpoint enable users to retrieve detailed information about the cancellation of a specific booking. Note that not all marketing carriers offer cancellations and that in some case, the cancellation `fee` do not reflect the flexibility of the terms and conditions due to technical reasons. The current workflow only allow a full (or partial) refund of the booking, no options for vouchers are currently available.

### HTTP Request

`GET api.distribusion.com/retailers/v3/cancellations/conditions`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`booking`           | true      | 22-letter alphanumeric lowercase code, provided on successful booking.

## Client errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.200.001 | API key is invalid or missing
400.800.041 | Booking not found
400.501.007 | Not implemented at Distribusion on cancellations#conditions
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable


## Create

```shell
curl -i --data '{"booking": "2PoHhetdJE02kRe1DhcgOg"}' 'https://api.distribusion.com/retailers/v3/cancellations/create' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "currency": "EUR"
  },
  "data": {
    "id": "vwmT-CI-keW3zWCc1F8jjQ",
    "type": "cancellations",
    "attributes": {
      "total_price": 1500,
      "fee": 1500,
      "total_refund": 0,
      "created_at": "2017-04-21T18:48"
    }
  }
}
```

This endpoint enables users to perform the cancellation of a specific booking.

### HTTP Request

`POST api.distribusion.com/retailers/v3/cancellations/create`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`booking`           | true      | 22-letter alphanumeric lowercase code, provided on successful booking.

## Client errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.200.001 | API key is invalid or missing
400.502.041 | Booking already cancelled
400.800.041 | Booking not found
400.501.008 | Not implemented at Distribusion on cancellations#create
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable
500.600.041 | Unique cancellation id cannot be generated

# Marketing Carriers

## Index

```shell
curl 'https://api.distribusion.com/retailers/v3/marketing_carriers' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "data": [
    {
      "id": "BLBS",
      "type": "marketing_carriers",
      "attributes": {
        "code": "BLBS",
        "trade_name": "Berlin Linien Bus"
      }
    },
    {
      "id": "MMAY",
      "type": "marketing_carriers",
      "attributes": {
        "code": "MMAY",
        "trade_name": "Mayer & Mayer"
      }
    },
    {
      "id": "POST",
      "type": "marketing_carriers",
      "attributes": {
        "code": "POST",
        "trade_name": "Postbus"
      }
    },

    .....
```

This endpoint provides a list of marketing carriers with their trade name and code.

### HTTP Request

`GET api.distribusion.com/retailers/v3/marketing_carriers`

### Errors

Error Code  | Meaning
----------- | -------
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable

## Show

```shell
curl 'https://api.distribusion.com/retailers/v3/marketing_carriers/JETB?locale=en&currency=EUR' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "locale": "en",
    "currency": "EUR"
  },
  "data": {
    "id": "JETB",
    "type": "marketing_carriers",
    "attributes": {
      "code": "JETB",
      "trade_name": "Jetbus",
      "legal_name": "RheinJet GmbH",
      "address": "Am Hövel 9, 40667 Meerbusch",
      "phone": "089 230 505 05",
      "fax": null,
      "customer_service_phone": "+49 89 230 505 05",
      "email": "service@rheinjet.de",
      "commercial_register": "Amtsgericht Neuss",
      "commercial_register_number": "HRB 13389",
      "vat_no": null,
      "authorised_representative": "Martin Michael",
      "white_label_logo": "data:image/png;base64, picture",
      "white_label_colour_code": null,
      "terms": "T&C",
      "flight_number_required": true,
      "booking_fee": 0,
      "cancellation_fee": 1500,
      "cancellation_cutoff": 720
    },
    "relationships": {
      "passenger_types": [
        {
          "data": {
            "id": "PNOR",
            "type": "passenger_types"
          }
        }
      ],
      "extra_types": []
    }
  },
  "included": [
    {
      "id": "PNOR",
      "type": "passenger_types",
      "attributes": {
        "code": "PNOR",
        "name": "Normal"
      }
    }
  ]
}
```

This endpoint enable users to retrieve detailed information about marketing carriers, such as passenger types, cancellations conditions, terms & conditions, etc.

### HTTP Request

`GET api.distribusion.com/retailers/v3/marketing_carriers/{code}`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`locale`            | true      | 2-letter alphanumeric lowercase code, according to ISO 639-1 standard.
`currency`          | true      | 3-letter alphanumeric uppercase code, according to ISO 4217 standard.

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.101.014 | Locale is invalid or missing
400.101.015 | Currency is invalid or missing
400.101.040 | Marketing carrier is invalid or missing
400.501.004 | Not implemented at Distribusion on bookings#form_schema
400.800.000 | Not Found
400.800.040 | Marketing carrier not found
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable

# Stations

```shell
curl 'https://api.distribusion.com/retailers/v3/stations?locale=en' --header 'Accept: application/vnd.api+json, Api-Key: H4gtqUn04qDh7mmZpFJYmmcW7JVw7hMIaCUcdWhZ'
```

> The above command returns JSON structured like this:

```json
{
  "jsonapi": {
    "version": "1.0"
  },
  "meta": {
    "locale": "en"
  },
  "data": [
    {
      "id": "DEEIBUFE",
      "type": "stations",
      "attributes": {
        "code": "DEEIBUFE",
        "name": "Eisenach Uferstraße",
        "street_and_number": "Uferstraße 40",
        "zip_code": "99817",
        "description": "Bus station is located at the intersection of Schillerstrasse and Gabelsberger Strasse, roughly 100 metres north-west of the Deutsche Bahn train station",
        "latitude": 50.978095,
        "longitude": 10.327165,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEEIB",
            "type": "cities"
          }
        }
      }
    },
    {
      "id": "DEBHEZOB",
      "type": "stations",
      "attributes": {
        "code": "DEBHEZOB",
        "name": "Bad Hersfeld ZOB",
        "street_and_number": "Bahnhofstraße 20",
        "zip_code": "36251",
        "description": "Bus station is located on the courtyard south-west of the Deutsche Bahn train station.",
        "latitude": 50.868954,
        "longitude": 9.715529,
        "time_zone": "Europe/Berlin"
      },
      "relationships": {
        "city": {
          "data": {
            "id": "DEBHE",
            "type": "cities"
          }
        }
      }

      .....
```

This endpoint enable users to retrieve detailed information about stations, such as name, id, GPS location, address and description.


### HTTP Request

`GET api.distribusion.com/retailers/v3/stations`

### URL Parameters

Parameter           | Mandatory | Description
------------------- | --------- | -----------
`locale`            | true      | 2-letter alphanumeric lowercase code, according to ISO 639-1 standard.

### Errors

Error Code  | Meaning
----------- | -------
400.000.000 | The provided json is invalid
400.100.000 | Request invalid
400.101.014 | Locale is invalid or missing
500.000.000 | Internal Server Error
500.100.000 | Service Unavailable
