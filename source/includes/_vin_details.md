# VIN Inquiry
  
## Fetch VIN Information
This route returns information about one or more vins.

> Example Response Body

```json
{
	"time": "2023-04-20T13:29:33.935Z",
	"vehicleCount": 2,
	"results": [
		{
			"vin": "KL1TD526X5B4XXXXX",
			"owner": {
				"name": "Jared",
				"address": "test address 1",
				"county": "Wichita"
			},
			"vehicle": {
				"year": "2005",
				"make": "example 1",
				"model": "example 1",
				"trim": "LS / Special Value",
				"titleProblem": true,
				"vehicleProblem": true,
				"vehicleOrTitleProblems": [
					"Title brand or other problem",
					"Accident",
					"Rebuilt"
				]
			},
			"registration": {
				"tagNumber": "example 1",
				"earliestDate": "2005-04-22",
				"latestDate": "2012-02-10",
				"expirationDate": "2016-04-25",
				"titleNumber": "10831838959165444"
			},
			"financial": {
				"titleIssueDate": "2012-02-10",
				"titledState": "MO"
			},
			"accident": {
				"count": "2",
				"earliestDate": "December 15, 1995 03:24:00",
				"lastReportedEvent": "string",
				"lastReportedAccidentDate": "string",
				"city": "string",
				"state": "string",
				"reportedAccidents": [
					{
						"accidentSeqNum": "0",
						"reportedDate": "December 17, 1995 03:24:00",
						"type": "string a",
						"city": "string a",
						"state": "string a"
					},
					{
						"accidentSeqNum": "1",
						"reportedDate": "December 15, 1995 03:24:00",
						"type": "string b",
						"city": "string b",
						"state": "string b"
					}
				]
			}
		},
		{
			"vin": "1GNFC130X8R2XXXXX",
			"owner": {
				"name": "John Doe",
				"address": "123 Main St.",
				"county": "New York"
			},
			"vehicle": {
				"year": "2016",
				"make": "BMW",
				"model": "4 Series",
				"trim": "435i",
				"titleProblem": false,
				"vehicleProblem": true,
				"vehicleOrTitleProblems": [
					"Title brand or other problem",
					"Accident",
					"Rebuilt"
				]
			},
			"registration": {
				"tagNumber": "ABC123",
				"earliestDate": "2015-06-01",
				"latestDate": "2022-03-15",
				"expirationDate": "2022-03-15",
				"titleNumber": "123456789"
			},
			"financial": {
				"titleIssueDate": "2022-03-15",
				"titledState": "NY"
			},
			"accident": {
				"count": "1",
				"earliestDate": "December 1, 2020 03:24:00",
				"lastReportedEvent": "Collision with animal",
				"lastReportedAccidentDate": "2020-09-30",
				"city": "New York",
				"state": "NY",
				"reportedAccidents": [
					{
						"accidentSeqNum": "1",
						"reportedDate": "December 1, 2020 03:24:00",
						"type": "Rear-end collision",
						"city": "Brooklyn",
						"state": "NY"
					}
				]
			}
		},
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

<aside class="warning">This feature is currently in the testing phase, and only five vins ('KL1TD526X5B4XXXXX', '1GNFC130X8R2XXXXX', 'JA7FJ23E5JJ0XXXXX', '3C3EL55H0TT2XXXXX', and '1FACP41E1LF1XXXXX') will return test data. All other vins will return a value of `No Info Found`</aside>
