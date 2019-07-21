# API slovak rail

base url `https://app.slovakrail.sk`

## Authentification:

Add header `x-api-key: PDh^2-$-M]8(dG8E+Q,FR}zsfz"Q~:N2pp\ykmg9ZEgKVrh42PHS?^sQ6<3;X,?-`. It looks that application "Ideme Vlakom" is using same api key for every user. Even when you are logged in. There is no handshake when you start application and the `x-api-key` is probably hardcoded to code.

### GET `api/supported-version/{versionNumber}`

returns `true` for `versionNumber=1` else `false`.
* response

```
    boolean
```

### GET `api/v1/init`

* response

```
{
	"ageCategories": [
        {
            "id": 102,
            "fromAge": 16,
            "toAge": 25,
            "description": "Mladý (16 - 25 r.)",
            "availableDiscounts": [{
                "id": 1,
                "description": "Bez zľavy",
                "documentRequired": false,
                "freeTransportAvailable": false
            }, {
                "id": 143,
                "description": "SLOVAK JUNI",
                "documentRequired": false,
                "freeTransportAvailable": false
            },
            ...
            ]
        },
        ...
    ],
	"trainTypes": [{
		"value": 1,
		"name": "Os",
		"description": "osobný vlak"
	}, {
		"value": 2,
		"name": "RR",
		"description": "regionálny rýchlik"
	},
    ...
    ],
	"placeAttributes": [{
		"description": "vozeň s kupé",
		"id": 1
	}, {
		"description": "vozeň veľkopriestorový",
		"id": 2
	},
    ...
    ]
}
```

### GET `api/v1/station/name/{stationPrefix}`

* query params
  
  * `maxCount` - results limit

* response
    
    ```
    [
        {
            "uicCode": "5613206",
            "name": "Bratislava hl.st.",
            "image": null,  <-- this is usually null
            "latitude": 48.157653,
            "longitude": 17.106339
        }, 
        ...    
    ]
    ```

* example

    ```
    https://app.slovakrail.sk/api/v1/station/name/bra?maxCount=100
    ```

### GET `api/v1/station/{stationName}/roster`

* query params
  
  * `departure` - boolean
  * `travelDate` - unix timestamp

* response

```
    [
        {
            "isDeparture": true,
            "station": "Humenné",
            "timestamp": 1563742080000,
            "train": {
                "type": 4,
                "typeList": [4],
                "number": "615",
                "name": "Zemplín",
                "features": [{
                    "id": 2,
                    "featureDescription": "Lôžkové vozne 1.trieda",
                    "featureName": "Lôžkové vozne 1.trieda",
                    "order": 0,
                    "reservationType": -1,
                    "reservationName": null,
                    "startStationIndex": null,
                    "stopStationIndex": null
                }, {
                    "id": 3,
                    "featureDescription": "Lôžkové vozne 2.trieda",
                    "featureName": "Lôžkové vozne 2.trieda",
                    "order": 0,
                    "reservationType": -1,
                    "reservationName": null,
                    "startStationIndex": null,
                    "stopStationIndex": null
                },
                ...
                ],
                "exceptions": [],
                "carrier": "Železničná spoločnosť Slovensko, a.s.",
                "trainDelay": null
            }
        },
        ...
    ]
```

* example

    ```
    https://app.slovakrail.sk/api/v1/station/BRATISLAVA/roster?departure=true&travelDate=1563739623309
    ```

### POST `api/v1/train/delay`

* body params
  
    ```
    [
        {
            "trainNumber": "3023",
            "travelDate": 1563715080000
        },
        ...
    ]
    ```

* response
    
    ```
    [
        {
            "trainNumber": "4611",
            "travelDate": 1563692940000,
            "trainDelay": {
                "previousStationUic": "56-134064-00",
                "previousStationName": "Tvrdošovce",
                "nextStationUic": "56-134262-00",
                "nextStationName": "Ľudovítov",
                "currentUic": "56-134163-00",
                "currentName": "Palárikovo",
                "delayMinutes": 1,
                "arrivedAtDestination": false,
                "timestamp": 1563697753000
            }
        },
        ...
    ]
    ```

### GET `api/v1/route`

* query params
  
    * `fromStation`
    * `toStation`
    * `departure`
    * `ageCategory`
    * `ageCategoryDiscount`
    * `travelDate`

* response

```
    [
        {
            "id": null,
            "length": 203,
            "duration": 162,
            "arrivalTimestamp": 1563767340000,
            "departureTimestamp": 1563757620000,
            "infoForCurrentConnection": null,
            "timeForCurrentConnection": null,
            "infoForNextConnection": 720903,
            "timeForNextConnection": 1563757620,
            "infoForPreviousConnection": 702629,
            "timeForPreviousConnection": 1563767340,
            "finalOfferExpiration": null,
            "routeSegments": [{
                "duration": 162,
                "length": 203,
                "departureTimestamp": 1563757620000,
                "arrivalTimestamp": 1563767340000,
                "trainStops": [
                    {
                        "arrivalTimestamp": 1563756660000,
                        "departureTimestamp": 1563757620000,
                        "trainStops": true,
                        "transferStation": false,
                        "trainStation": {
                            "uicCode": "5617915",
                            "name": "Žilina",
                            "image": null,
                            "latitude": 49.21945,
                            "longitude": 18.7408
                        }
                    },
                    ...
                ],
                "availableTicketClasses": null,
                "hasReservation": null,
                "ticketClass": null,
                "train": {
                    "type": 4,
                    "typeList": [4],
                    "number": "614",
                    "name": "Zemplín",
                    "features": [
                        {
                            "id": 2,
                            "featureDescription": "Lôžkové vozne 1.trieda",
                            "featureName": "Lôžkové vozne 1.trieda",
                            "order": 19,
                            "reservationType": -1,
                            "reservationName": null,
                            "startStationIndex": -1,
                            "stopStationIndex": -1
                        },
                        ...
                    ],
                    "exceptions": [],
                    "carrier": "Železničná spoločnosť Slovensko, a.s.",
                    "trainDelay": null
                },
                "finalStation": {
                    "uicCode": "5614616",
                    "name": "Bratislava-Nové Mesto",
                    "image": null,
                    "latitude": 48.167152,
                    "longitude": 17.136849
                }
            }],
            "routeSelfRefs": [{
                "selfRef": 364
            }, {
                "selfRef": 371938256
            }, {
                "selfRef": 231
            }, {
                "selfRef": 3
            }, {
                "selfRef": 240445
            }, {
                "selfRef": 720903
            }, {
                "selfRef": 702629
            }],
            "passengers": []
        },
        ...
    ]
```

* example

    ```
    /api/v1/route/?fromStation=5617915&toStation=BRATISLAVA&departure=true&ageCategory=102&ageCategoryDiscount=1&travelDate=1563740034000
    ```

### GET `api/v1/route/pricing`

* query params
  
    * `travelDate`
    * `selfRef`
    * `selfRef`
    * `selfRef`
    * `selfRef`
    * `selfRef`
    * `selfRef`
    * `selfRef`
    * `ageCategory`
    * `ageCategoryDiscount`
    * `freeTransportDiscount`
    * `order`

* response

```
{
	"segmentOptions": [{
		"segmentOrder": 0,
		"segmentClassOptions": [
			{
				"trainClass": 1,
				"reservationOptions": [{
					"reservation": -1,
					"unavailable": false
				}]
			},
			...
		]
	}],
	"optionPricings": [
		{
			"segments": [{
				"segmentOrder": 0,
				"trainClass": 1,
				"reservationOption": -1
			}],
			"passengers": [{
				"passengerOrder": 0,
				"offer": "{\"segments\":[{\"reservationOptions\":[],\"availability\":\"AVAILABLE\",\"classId\":2,\"reservationMandatory\":false,\"supplementPrice\":0.00}],\"calculation\":\"1-1-1-1-1-2-2-0-0\",\"groupedOffer\":false,\"itinerary\":\"ONEWAY\",\"passengers\":[{\"group\":null,\"passengerType\":\"PERSON\",\"person\":{\"ageCategory\":102,\"discountTypeId\":1,\"description\":null,\"reference\":1},\"service\":null}],\"price\":9.38,\"priceType\":\"PAYED_FARE\",\"product\":1}",
				"price": 9.38,
				"ticketRequired": true,
				"ageCategoryId": 102,
				"ageCategoryDiscountId": 1,
				"freeTransportDiscount": false
			}]
		},
		...
	]
}
```

* example

    ```
    /api/v1/route/pricing?travelDate=1563757620000&selfRef=364&selfRef=372193256&selfRef=231&selfRef=3&selfRef=240445&selfRef=720903&selfRef=702629&ageCategory=102&ageCategoryDiscount=1&freeTransportDiscount=false&order=0
    ```

### POST `api/v1/route/final-pricing`

* body params
  
    ```
    {
        "allowedPaymentModes": [],
        "arrivalTimestamp": 1563767340000,
        "departureTimestamp": 1563757620000,
        "duration": 162,
        "finalOfferExpiration": 0,
        "id": 0,
        "infoForCurrentConnection": 0,
        "infoForNextConnection": 0,
        "infoForPreviousConnection": 0,
        "isSummaryExpanded": false,
        "length": 203,
        "loaderResponseSize": 0,
        "loaderType": 0,
        "passengers": [{
            "ageCategoryDiscountId": 1,
            "ageCategoryId": 102,
            "firstName": "Test",
            "freeTransportDiscount": false,
            "isUser": true,
            "lastName": "Test",
            "offer": "{\"segments\":[{\"reservationOptions\":[],\"availability\":\"AVAILABLE\",\"classId\":2,\"reservationMandatory\":false,\"supplementPrice\":0.00}],\"calculation\":\"1-1-1-1-1-2-2-0-0\",\"groupedOffer\":false,\"itinerary\":\"ONEWAY\",\"passengers\":[{\"group\":null,\"passengerType\":\"PERSON\",\"person\":{\"ageCategory\":102,\"discountTypeId\":1,\"description\":null,\"reference\":1},\"service\":null}],\"price\":9.38,\"priceType\":\"PAYED_FARE\",\"product\":1}",
            "order": 0,
            "priceInLong": 938,
            "reservation": [{
                "isEdited": true,
                "placeNumber": -1,
                "relativePosition": 8,
                "reservationCategory": -1,
                "reservationType": 0,
                "segmentOrder": 0,
                "trainClass": 1,
                "wagonNumber": -1
            }],
            "ticketRequired": true,
            "zsskRegistrationNumber": 0
        }],
        "priceFrom": 0,
        "routeSegments": [{
            "arrivalTimestamp": 1563767340000,
            "departureTimestamp": 1563757620000,
            "duration": 162,
            "finalStation": {
                "latitude": 48.167152,
                "longitude": 17.136849,
                "name": "Bratislava-Nové Mesto",
                "uicCode": "5614616"
            },
            "length": 203,
            "nextTrainStops": [{
                "arrivalTimestamp": 1563771060000,
                "departureTimestamp": 0,
                "state": 0,
                "trainDelay": 0,
                "trainStation": {
                    "latitude": 48.167152,
                    "longitude": 17.136849,
                    "name": "Bratislava-Nové Mesto",
                    "uicCode": "5614616"
                },
                "trainStops": true,
                "transferStation": false
            }],
            "previousTrainStops": [
                {
                    "arrivalTimestamp": 0,
                    "departureTimestamp": 1563738840000,
                    "state": 0,
                    "trainDelay": 0,
                    "trainStation": {
                        "latitude": 48.932454,
                        "longitude": 21.907892,
                        "name": "Humenné",
                        "uicCode": "5616040"
                    },
                    "trainStops": true,
                    "transferStation": false
                },
                ...
            ],
            "train": {
                "carrier": "Železničná spoločnosť Slovensko, a.s.",
                "exceptions": [],
                "features": [
                    {
                        "featureDescription": "Lôžkové vozne 1.trieda",
                        "featureName": "Lôžkové vozne 1.trieda",
                        "id": 2,
                        "order": 19,
                        "reservationType": -1,
                        "startStationIndex": -1,
                        "stopStationIndex": -1
                    },
                    ...
                ],
                "name": "Zemplín",
                "number": "614",
                "trainDelay": {
                    "arrivedAtDestination": false,
                    "currentName": "Michalovce",
                    "currentUic": "56-167304-00",
                    "delayMinutes": 3,
                    "nextStationName": "Bánovce nad Ondavou",
                    "nextStationUic": "56-166900-00",
                    "previousStationName": "Strážske",
                    "previousStationUic": "56-160101-00",
                    "timestamp": 1563740430000
                },
                "typeList": [4]
            },
            "trainStops": [
                {
                    "arrivalTimestamp": 1563756660000,
                    "departureTimestamp": 1563757620000,
                    "state": 0,
                    "trainDelay": 0,
                    "trainStation": {
                        "latitude": 49.21945,
                        "longitude": 18.7408,
                        "name": "Žilina",
                        "uicCode": "5617915"
                    },
                    "trainStops": true,
                    "transferStation": false
                },
                ...
            ]
        }],
        "routeSelfRefs": [{
            "selfRef": 364
        }, {
            "selfRef": 372193256
        }, {
            "selfRef": 231
        }, {
            "selfRef": 3
        }, {
            "selfRef": 240445
        }, {
            "selfRef": 720903
        }, {
            "selfRef": 702629
        }],
        "state": 0,
        "tickets": [],
        "timeForCurrentConnection": 0,
        "timeForNextConnection": 0,
        "timeForPreviousConnection": 0,
        "type": "FULL_CUSTOMER_ACCOUNT"
    }
    ```

* response

```
    {
        "allowedPaymentModes": [{
            "value": 4
        }, {
            "value": 3
        }, {
            "value": 0
        }, {
            "value": 2
        }, {
            "value": 1
        }],
        "expiration": 1563741780000,
        "passengers": [{
            "passengerOrder": 0,
            "offer": "{\"priceOfferExpiration\":\"2019-07-21T20:43:00.511Z\",\"priceOfferId\":\"159a2cc2-92b2-41d4-9760-d74fe2dcab5e\",\"segments\":[{\"availability\":null,\"reservationOption\":null,\"classId\":2,\"reservationMandatory\":false,\"supplementPrice\":0.00}],\"clientReference\":null,\"calculation\":\"1-1-1-1-1-2-2-0-0\",\"groupedOffer\":false,\"itinerary\":\"ONEWAY\",\"passengers\":[{\"group\":null,\"passengerType\":\"PERSON\",\"person\":{\"ageCategory\":102,\"discountTypeId\":1,\"description\":null,\"reference\":1},\"service\":null}],\"price\":9.38,\"priceType\":\"PAYED_FARE\",\"product\":1}",
            "price": 9.38,
            "ticketRequired": true,
            "ageCategoryId": 102,
            "ageCategoryDiscountId": 1,
            "freeTransportDiscount": false
        }]
    }
```

### POST `api/v1/order`

* body params
  
    ```
    {
        "creditDiscountLong": 0,
        "email": "aaa@bbb.com",
        "paymentType": "CARD",
        "routes": [{
            "allowedPaymentModes": [{
                "value": 4
            }, {
                "value": 3
            }, {
                "value": 0
            }, {
                "value": 2
            }, {
                "value": 1
            }],
            "arrivalTimestamp": 1563767340000,
            "departureTimestamp": 1563757620000,
            "duration": 162,
            "finalOfferExpiration": 1563741780000,
            "id": 1,
            "infoForCurrentConnection": 0,
            "infoForNextConnection": 0,
            "infoForPreviousConnection": 0,
            "isSummaryExpanded": false,
            "length": 203,
            "loaderResponseSize": 0,
            "loaderType": 0,
            "passengers": [{
                "ageCategoryDiscountId": 1,
                "ageCategoryId": 102,
                "firstName": "Test",
                "freeTransportDiscount": false,
                "isUser": true,
                "lastName": "Test",
                "offer": "{\"priceOfferExpiration\":\"2019-07-21T20:43:00.511Z\",\"priceOfferId\":\"159a2cc2-92b2-41d4-9760-d74fe2dcab5e\",\"segments\":[{\"availability\":null,\"reservationOption\":null,\"classId\":2,\"reservationMandatory\":false,\"supplementPrice\":0.00}],\"clientReference\":null,\"calculation\":\"1-1-1-1-1-2-2-0-0\",\"groupedOffer\":false,\"itinerary\":\"ONEWAY\",\"passengers\":[{\"group\":null,\"passengerType\":\"PERSON\",\"person\":{\"ageCategory\":102,\"discountTypeId\":1,\"description\":null,\"reference\":1},\"service\":null}],\"price\":9.38,\"priceType\":\"PAYED_FARE\",\"product\":1}",
                "order": 0,
                "priceInLong": 938,
                "reservation": [{
                    "isEdited": false,
                    "placeNumber": -1,
                    "relativePosition": 8,
                    "reservationCategory": -1,
                    "reservationType": 0,
                    "segmentOrder": 0,
                    "trainClass": 1,
                    "wagonNumber": -1
                }],
                "ticketRequired": true,
                "zsskRegistrationNumber": 0
            }],
            "priceFrom": 0,
            "routeSegments": [{
                "arrivalTimestamp": 1563767340000,
                "departureTimestamp": 1563757620000,
                "duration": 162,
                "finalStation": {
                    "latitude": 48.167152,
                    "longitude": 17.136849,
                    "name": "Bratislava-Nové Mesto",
                    "uicCode": "5614616"
                },
                "length": 203,
                "nextTrainStops": [{
                    "arrivalTimestamp": 1563771060000,
                    "departureTimestamp": 0,
                    "state": 0,
                    "trainDelay": 0,
                    "trainStation": {
                        "latitude": 48.167152,
                        "longitude": 17.136849,
                        "name": "Bratislava-Nové Mesto",
                        "uicCode": "5614616"
                    },
                    "trainStops": true,
                    "transferStation": false
                }],
                "previousTrainStops": [
                    {
                        "arrivalTimestamp": 0,
                        "departureTimestamp": 1563738840000,
                        "state": 0,
                        "trainDelay": 0,
                        "trainStation": {
                            "latitude": 48.932454,
                            "longitude": 21.907892,
                            "name": "Humenné",
                            "uicCode": "5616040"
                        },
                        "trainStops": true,
                        "transferStation": false
                    },
                    ...
                ],
                "train": {
                    "carrier": "Železničná spoločnosť Slovensko, a.s.",
                    "exceptions": [],
                    "features": [
                        {
                            "featureDescription": "Lôžkové vozne 1.trieda",
                            "featureName": "Lôžkové vozne 1.trieda",
                            "id": 2,
                            "order": 19,
                            "reservationType": -1,
                            "startStationIndex": -1,
                            "stopStationIndex": -1
                        },
                        ...
                    ],
                    "name": "Zemplín",
                    "number": "614",
                    "trainDelay": {
                        "arrivedAtDestination": false,
                        "currentName": "Michalovce",
                        "currentUic": "56-167304-00",
                        "delayMinutes": 3,
                        "nextStationName": "Bánovce nad Ondavou",
                        "nextStationUic": "56-166900-00",
                        "previousStationName": "Strážske",
                        "previousStationUic": "56-160101-00",
                        "timestamp": 1563740430000
                    },
                    "typeList": [4]
                },
                "trainStops": [
                    {
                        "arrivalTimestamp": 1563756660000,
                        "departureTimestamp": 1563757620000,
                        "state": 0,
                        "trainDelay": 0,
                        "trainStation": {
                            "latitude": 49.21945,
                            "longitude": 18.7408,
                            "name": "Žilina",
                            "uicCode": "5617915"
                        },
                        "trainStops": true,
                        "transferStation": false
                    },
                    ...
                ]
            }],
            "routeSelfRefs": [{
                "selfRef": 364
            }, {
                "selfRef": 372193256
            }, {
                "selfRef": 231
            }, {
                "selfRef": 3
            }, {
                "selfRef": 240445
            }, {
                "selfRef": 720903
            }, {
                "selfRef": 702629
            }],
            "state": 1,
            "tickets": [],
            "timeForCurrentConnection": 0,
            "timeForNextConnection": 0,
            "timeForPreviousConnection": 0,
            "type": "FULL_CUSTOMER_ACCOUNT"
        }],
        "standardDiscountLong": 0
    }
    ```

* response

```
{
	"expirationTimestamp": 1563742300000,
	"orderId": "1113104055798555",
	"price": 9.38,
	"error": -1,
	"paymentOperationId": null,
	"maxAttemptsToPay": null,
	"creditDiscount": null,
	"standardDiscount": null,
	"discountOfferId": null
}
```

### GET `api/v1/payment`

* query params

    * `orderId`
    * `expiration`
    * `price`
    * `email`
    * `paymentMode`

* response
    
    ```
    {
        "expirationTimestamp": 1563742300000,
        "orderId": "1113104055798555",
        "paymentEmail": "aaa@bbb.com",
        "paymentMode": {
            "value": 3
        },
        "paymentURL": "https://3dsecure.gpwebpay.com/pgw/order.do?MERCHANTNUMBER=3041870&OPERATION=CREATE_ORDER&ORDERNUMBER=88125030451&AMOUNT=938&CURRENCY=978&DEPOSITFLAG=1&MERORDERNUM=8812503045&URL=https://app.slovakrail.sk/api/v1/gpw/response&DESCRIPTION=&MD=1_1563742300000_9.38_emlwcGVyc2subW1AZ21haWwuY29t_1113104055798555&DIGEST=KXY%2BS/D57GHG5ORHxeR1Fu0n1ZeoWVUtbUEoKMooeOA/DkSuomfbH0C%2Bm/J/vS10cSDn1pJXJftkItAVND0qdX7gXf5QSmixe1niUWbYLOf2lISE6jwA6fvyyKxIHA9eud4236lqFpybBtgIP5A3UhIqAakES8Z2JtZ6QfuO9AbqKnkggmaJCFgiHQsrdI%2B2Pyd88nexRtv74cZTaRpwMMjGYmWTIgie3e%2B57LaIGpuP2ZA/EAHcnqzKwZpfK4/wjGK/oIldxajzeKy3RF3Wc3HVzYcZaFpnNE4fe/OYYqmzQOrdptEPXAHdWn4jZRW8lQaso/vvXL9eJPpmCL3ttg%3D%3D",
        "price": "9.38",
        "ss": "1",
        "vs": "8812503045"
    }
    ```

* example

    ```
    https://app.slovakrail.sk/api/v1/payment?orderId=1113104055798555&expiration=1563742300000&price=9.38&email=aaa%40bbb.com&paymentMode=3
    ```
