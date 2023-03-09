# VIN Inquiry
  
## Fetch VIN Information
This route returns information about one or more vins.

> Example Response Body

```json
{
	"time": "2023-03-09T16:30:01.644Z",
	"vehicleCount": 2,
	"results": [
		{
			"vin": "JN8AZ28R49T122921",
			"owner": {
				"name": "JOHN LEE DOE",
				"address": "123 MAIN STREET, DALLAS, TX 75204-1407",
				"county": "DALLAS"
			},
			"vehicle": null,
			"financial": null,
			"accident": null,
			"registration": {
				"recordType": "CURRENT",
				"name": "JOHN LEE DOE",
				"address": "123 MAIN STREET, DALLAS, TX 75204-1407",
				"county": "DALLAS",
				"tagNumber": "OOOXXX",
				"earliestDate": "XX/XX/XXXX",
				"latestDate": "XX/XX/XXXX",
				"expirationDate": "XX/XX/XXXX",
				"previousTagNumber": "OOOXXX"
			}
		},
		{
			"vin": "JH4DA9440NS003801",
			"owner": {
				"name": "FIRST MIDDLE LAST",
				"address": "321 MAIN STREET, DALLAS, TX 75204-1407",
				"county": "DALLAS"
			},
			"vehicle": null,
			"financial": null,
			"accident": null,
			"registration": {
				"recordType": "CURRENT",
				"name": "FIRST MIDDLE LAST",
				"address": "321 MAIN STREET, DALLAS, TX 75204-1407",
				"county": "DALLAS",
				"tagNumber": "OOOXXX",
				"earliestDate": "XX/XX/XXXX",
				"latestDate": "XX/XX/XXXX",
				"expirationDate": "XX/XX/XXXX",
				"previousTagNumber": "OOOXXX"
			}
		}
	]
}
```
### HTTP Request

`POST https://xapi.lossexpress.com/vin-check`

### Request Body
Body Parameter | Description | Required
-------------- | ----------- | ---------
vins | An array of 17-digit VINs | Y
requestedData | An array of data types to fetch* | Y

*Accepted data types are `"OWNER", "VEHICLE", "ACCIDENT", "FINANCIAL", "REGISTRATION"`

<aside class="warning">This feature is currently in the testing phase, and only two vins (`JN8AZ28R49T122921` and `JH4DA9440NS003801`) will return test data. All other vins will return a value of `No Info Found`</aside>
