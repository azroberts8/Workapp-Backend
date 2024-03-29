# Communication Specification

## Preface

### Minimal vs Full Types
Many entities including users, organizations, and schools feature minimal and full variations of their JSON objects. Minimal types feature the least amount of information to display the entity as a list item while full types contain all the information available about an entity for displaying a full page or card about the entity. *See [Minimal Types](#minimal-types) section for more information.*

***Example:*** When users view their feed of projects/jobs they would like to see information such as the name and logo of the company that made the post. However, they do not need to see the entire company's bio or other opportunities that the company offers unless they are viewing the company's profile page. In this case a minimal representation of the company will be embedded in the project/job card and the full version will be sent if the user views the company page.

### Responses
All JSON responses implement the following structure:
```json
{
    "status": "success",
    "message": "",
    "payload": {}
}
```
**status:** indicates whether the request was fulfilled successfully; possible values `success` || `error`  
**message:** provides details in the event of an error; only populated when status=`error`  
**payload:** contains the requested data; only populated when status=`success`

### IDs
All IDs that uniquely identify users, schools, and jobs are 9 character, case insensitive strings where the first character identifies whether it is a **U**ser, **S**chool or **J**ob. The remaining 8 characters exist within the range 0-F (like hexidecimals) and identify the unique entity. *I will later provide regex for validating these IDs*

## Minimal Types
Minimal types *do not* feature explicit API endpoints to access them. Rather, minimal types are intended to be embedded as list items within other endpoints. 
### Minimal Applicant User
```json
{
    "userId": "u12345678",
    "userType": "applicant",
    "username": "azroberts",
    "displayName": "Andrew Roberts",
    "logo": "/link/to/profile/photo",
    "tagline": "Innovative tech bro changing the world",
    "rating": 3.8,
    "fieldOfStudy": "computer science",
    "location": "Middletown, DE",
}
```

### Minimal Company/Organization User
```json
{
    "userId": "u12345678",
    "userType": "organization",
    "username": "lutron",
    "displayName": "Lutron Electronics",
    "logo": "/link/to/logo",
    "tagline": "Providing the highest quality lighting solutions",
    "rating": 4.8,
    "industry": "consumer electronics",
    "orgType": "private corporation"
}
```

### Minimal School
```json
{
    "schoolId": "s12345678",
    "schoolName": "University of Delaware",
    "schoolType": "university",
    "schoolLogo": "/link/to/logo",
    "schoolLocation": "Newark, DE"
}
```

### Minimal Job/Project Posting
```json
{
    "listingId": "j12345678",
    "listingTitle": "Embedded Software Engineer Summer Internship",
    "listingType": "internship",
    "organization": {}  // minimal company user
}
```

## Full Types Without Endpoints
### Full Job/Project Posting
```json
{
    "jobId": "j12345678",
    "jobName": "Embedded Software Summer Internship",
    "jobType": "Internship",
    "compensated": "hourly",
    "compensation": 30.00,
    "employer": {},  // minimal company/organization user
    "locations": ["Austin, TX", "Coopersburg, PA"],
    "workType": "Embedded Software Engineering",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "posted": "2023-06-30",
    "expires": "2023-12-28"
}
```

## Authentication

## Profile
### Get Profile Details
**Endpoint:** `/profile/{username}`  
**Method:** GET  
**Headers:** *Optional:* `Authorization: Bearer {user-jwt-token}`  
**Description:** Retrieves all information pertaining to a specified user. If no authorization header is provided or the profile does not belong to the authorized user, privated fields will be ommitted in the response.  

**Full Applicant Response**
```json
{
    "status": "success",
    "message": "",
    "payload": {
        "userId": "u12345678",
        "userType": "applicant",
        "username": "azroberts",
        "displayName": "Andrew Roberts",
        "logo": "/link/to/profile/photo",
        "tagline": "Innovative tech bro changing the world",
        "rating": 3.8,
        "fieldOfStudy": "computer science",
        "location": "Middletown, DE",
        "email": "andrew@example.com",
        "privateEmail": false,
        "bio": "orem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
        "locationPreferences": ["Austin, TX", "San Francisco, CA", "Tampa, FL"],
        "industryPreferences": ["consumer electronics", "aerospace", "software"],
        "rolePreferences": ["software engineering", "embedded firmware engineering"],
        "webpages": [
            {
                "label": "Personal Portfolio",
                "link": "https://azroberts.com"
            }
        ],
        "studies": [
            {
                "school": {  // minimal school example
                    "schoolId": "s12345678",
                    "schoolName": "University of Delaware",
                    "schoolType": "university",
                    "schoolLogo": "/link/to/logo",
                    "schoolLocation": "Newark, DE"
                },
                "majors": ["B.S. Computer Science"],
                "minors": ["Cybersecurity"],
                "concentrations": ["Systems & Networks"],
                "gpa": 3.60,
                "privateGpa": false,  // hides GPA
                "graduated": false,
                "currentlyEnrolled": true,
                "enrolledSince": "2021-01-05",
                "completed": "2024-12-15",
                "privateDates": false,  // hides enrollment dates
                "privateStudy": false  // hides this school from experience list
            },
            {
                "school": {},  // minimal school (highschool)
                "majors": [],
                "minors": [],
                "concentrations": [],
                "gpa": 3.8,
                "privateGpa": false,
                "graduated": true,
                "currentlyEnrolled": false,
                "enrolledSince": "2015-08-15",
                "completed": "2019-06-05",
                "privateDates": false,
                "privateStudy": false
            }
        ],
        "internalExperiences": [
            {
                "listing": {},  // minimal project listing
                "completed": "2023-08-15",
                "duration": "11 weeks",
                "ratingRecieved": 4.2,
                "noteFromOrg": "Andrew performed alright during this internship program.",
                "privateExperience": false  // hides the experience from the users ublic profile
            }
        ],
        "externalWork": [],
        "externalProjects": [],
        "hobbies": ["photography", "trail biking"]
    }
}
```

### Full Organization Response
```json
{
    "status": "success",
    "message": "",
    "payload": {
        "userId": "u12345678",
        "userType": "organization",
        "username": "lutron",
        "displayName": "Lutron Electronics",
        "logo": "/link/to/company/logo",
        "tagline": "Providing the highest quality lighting solutions",
        "rating": 4.8,
        "industry": "consumer electronics",
        "orgType": "private corporation",
        "organizationSize": 15000,
        "organizationSizeLabel": "medium",
        "webpage": "https://lutron.com",
        "bio": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
        "locations": ["Austin, Tx", "Philadelphia, PA", "Coopersburgh, PA"],
        "jobs": [
            {}  // minimal job/project postings
        ]
    }
}
```

### Update Profile Details


## User Feed

### Get User Feed
**Endpoint:** `/feed`  
**Method:** GET  
**Headers:** `Authorization: Bearer {user-jwt-token}`  
**Description:** Retrieves a list of jobs/projects for user identified in the authorization token. Considering the user has a watch list of *N* items, the first *N* items of their feed will be their watch list and proceeding items will be positions not yet seen by the user. If no `start` property is defined in the request, the response will be centered on the point in the feed where the watched positions end and new positions begin.  

**Request**
 
| Parameter | Required | Type | Default | Description |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| `count` | no| int | 10 | Number of jobs/projects to fetch per page |
| `start` | no | int | arbitrary | Defines the feed index item to start at; eg. count=5 start=3 returns feed items 3, 4, 5, 6, and 7 |
| `minimal` | no | bool | false | Indicates whether to use minimal or full job/project listings |

**Response**
```json
{
    "status": "success",
    "message": "",
    "payload": {
        "start": 0,
        "count": 5,
        "watched": 3,  // indicates that the first 3 feed items are from watched list
        "endOfFeed": true,
        "jobs": [
            {},  // full or minimal job/project listings
            {},
            {},
            {},
            {}
        ]
    }
}
```

### User apply/dismiss or add job watch list
**Endpoint:** `/feed`  
**Method:** POST  
**Headers:** `Authorization: Bearer {user-jwt-token}`  
**Description:** Used to indicate whether an applicant applies to or dismisses a position.

**Request**
| Parameter | Required | Type | Default | Description |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| `jobId` | yes | int | - | Unique ID of the job position the user if applying to or dismissing |
| `action` | yes | string | - | Indicates whether the user is applying to or dismissing the position; values are `apply`, `dismiss`, and `watch` |

**Response**
```json
{
    "status": "success",
    "message": ""
}
```


## Employer Feed

## Project