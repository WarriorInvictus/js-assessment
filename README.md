# Technical Assessment

This technical assessment should be time-boxed to 2-3 hours maximum. There are no trick questions; this test is meant to be used as a high-level view of your understanding of React and javascript.

## Background

One of the factors that impact the auto insurance premiums is the vehicles included in the policy. During the quote process, users must select their vehicle and submit to our system to receive a valid price estimate.

We need to make the vehicle selection process easy for users but standardized for our rater to consume the data. The rater accepts plain text strings and requires a `year`, `make`, and `model` for each vehicle.

Your goal is to create a simple React (preferred, but Angular or Vue are acceptable) apps that will allow a user to add vehicles to a quote. You'll need to connect to the NHTSA API to fetch vehicles makes and models to add a vehicle.

## What to do

To complete the assessment, build a React app that satisfies the following two user stories. You can use this react app base or start a new one if you'd like. Make sure to use Git to save your progress. Once completed, zip of the entire directory and send back to our team for review.

There are two user stories and some API documentation, including schema and request/response sample. These should be helpful in writing your code. Don't hesitate to reach out with any questions.

## User Stories

### 1. As a User with a draft Quote, I can add a Vehicle to my Quote by selecting the year, make, and model from a list of dropdowns.

**Acceptance Criteria**

- [ ] Verifies `year` is required
- [ ] Verifies can select a year from 1980 to 2021
- [ ] Verifies `make` is required
- [ ] Verifies can select a `make` from a dropdown menu
- [ ] Verifies `make`s are ordered alphabetically
- [ ] **UPGRADE** Verifies I can type to filter down the list of `make`s
- [ ] Verifies I can select a `model` based on the `year` and `make`
- [ ] Verifies `model` is required
- [ ] **UPGRADE** Verifies I can type to filter down the list of `model`s

To populate your `make` and `model` dropdowns, you'll need to connect to the [NHTSA API](https://vpic.nhtsa.dot.gov/api/). Their API is completely free and does not require authentication. Here are the URLs that you can use to make the request:

**Get Makes**
The NHTSA has an API for getting vehicle makes; since their API includes more than just cars, we'll want to use their "Get Vehicle Makes" API. Here is a [sample payload](https://vpic.nhtsa.dot.gov/api/vehicles/GetMakesForVehicleType/car?format=json) for reference.

```$
curl -X GET -i https://vpic.nhtsa.dot.gov/api/vehicles/GetMakesForVehicleType/car?format=json
```
**Get Models**
When a user selects their make, you'll need to filter down the models dropdown to only show models for the select year and make. The NHTSA has an endpoint called GetModelsForMake that will give you the results you need. Check out this [sample response](https://vpic.nhtsa.dot.gov/api/vehicles/getmodelsformake/acura?format=json) for reference.

```$
curl -X GET -i https://vpic.nhtsa.dot.gov/api/vehicles/getmodelsformake/acura?format=json
```

_Make sure you swap out `acura` in that above query with the name of the make from the previous request for vehicle makes, e.g. `toyota`, `honda`, etc._


### 2. As a User with a Vehicle added to my draft Quote, I can add another Vehicle

**Acceptance Criteria**

- [ ] Verifies can add up to 3 vehicles
- [ ] Verifies can add the same vehicle type

_Any acceptance criteria labeled **UPGRADE** is not required. If you have the time and want to add the additional functionality, then you are welcome to. But please don't spend more than 3 hours total on this assessment._


## Project Setup

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `yarn server`

`json-server` is set up to create a fake REST API for you to test against. If you run `yarn server` you'll see a response like this:

```bash
yarn run v1.22.5
$ json-server --watch server/db.json --port 5000

\{^_^}/ hi!

Loading server/db.json
Done

Resources
http://localhost:5000/quotes
http://localhost:5000/vehicles

Home
http://localhost:5000
```

You can test out the API endpoints in the browser or REST client with:

```bash
curl -i http://localhost:5000/quotes
curl -i http://localhost:5000/vehicles
```

### API Endpoints

There are a few endpoints that you can work with to complete the assessment. They include the following:

#### Get Quote
You can fetch the existing quote to get started.

```bash
$ curl -i http://localhost:5000/quotes/1
```

Response:
```json
{
  "id": 1,
  "status": "draft",
  "zip_code": "60622",
  "email_address": "john.doe@example.com",
  "homeowner": true,
  "currently_insured": true
}
```

#### Create Vehicle

To create a vehicle, you can hit the following URL:

```bash
$ curl --request POST \
  --url http://localhost:5000/quotes/1/vehicles \
  --header 'content-type: application/json' \
  --data '{
    "quote_id": 1,
    "year": "2017",
    "make": "ACURA",
    "model": "RDX",
    "trim": null,
    "registered_state": "IL",
    "use_code": "commuting"
  }
  '
```

Response will look like this;

```json
{
  "id": 1,                  // Unique identifier that's automatically created
  "quote_id": 1,            // ID of the quote that the vehicle is being added to
  "make": "Acura",          // Make (e.g. Acura, Toyota, etc)
  "model": "MDX",           // Model of the vehicle
  "year": 2017,             // Year of the vehicle
  "trim": null,             // Trim (can be null)
  "registered_state": "IL", // State the vehicle is registered (e.g. IL, NY, CA, etc)
  "use_code": "commuting",  // Use code can be `commuting`, `errands`, or `business`
}
```
