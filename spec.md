# Communication Specification

## Authentication

## Applicant Profile

## Employer Profile


## User Feed

### Get User Feed
**Endpoint:** `/feed`
**Method:** GET
**Headers:** `Authorization: Bearer {user-jwt-token}`
**Description:** Retrieves a list of jobs/projects for user identified in the authorization token

**Request**
 
| Parameter | Required | Type | Default | Description |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| count | no| int | 10 | Number of jobs/projects to fetch per page |
| page | no | int | 0 | Used to request more projects; eg. count=10 page=0 returns first 10 projects, page=1 returns the next 10 |

**Response**
```json
{
	"page": 0,
	"count": 2,
	"endOfFeed": true,
	"jobs": [
		{
			"id": 58695995,  // uniquely identifies job 
			"jobName": "Add Bluetooth to Corn Robot",
			"jobType": "Project",
			"employerName": "University of Delaware Agriculture",
			"employerId": 83028594,
			"location": "Newark, DE",
			"industry": "Agricultural Research",
			"roleName": "Embedded Software Engineering",
			"posted": "2023-06-30T12:53:37.738Z",
			"expires": "2023-12-30T12:53:37.738Z"
		},
		{
			"id": 58628990,
			"jobName": "Embedded Software Summer Internship",
			"jobType": "Internship",
			"employerName": "Lutron Electronics",
			"employerId": 39105603,
			"location": "Austin, TX",
			"industry": "Consumer Electronics",
			"roleName": "Embedded Software Engineering",
			"posted": "2023-06-30T12:53:37.738Z",
			"expires": "2023-12-30T12:53:37.738Z"
		}
	]
}
```


## Employer Feed

## Project