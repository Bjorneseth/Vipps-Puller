# What problem do i want to solove

Vipps is a payment service predominantly tailored for the Norwegian market. It significantly simplifies monetary transactions between individuals and businesses. To initiate a transfer, one only needs the recipient's phone number or the business's unique Vipps number. Each transaction allows the sender to attach a message. Importantly, all Vipps accounts are linked to a Norwegian ID, ensuring that each account is connected to a verified personal name. The name is also visible to the money's recipient.

Previously, the sender's name and any attached message were not accessible through the Vipps API. However, this has changed with the recent introduction of a GDPR-compliant feature. Before this update, integrating Vipps into our system was not a viable option due to our need for this specific information. Now, with the new GDPR-compliant attributes, we can implement Vipps, aligning with the benefits we seek.

# Pre planning

## File structure

```
├── vipps
│ ├── client.go     // Vipps API-client
│ ├── client_test.go
│ ├── models.go     // Vipps datamodels
├── visma
│ ├── client.go     // Visma API-client
│ ├── client_test.go
│ ├── models.go     // Visma API-models
├── converter
│ ├── converter.go  // Logic for the conversion between Vipps and Visma
│ ├── converter_test.go
├── storage
│ ├── date.txt     // The last successfully date we pulled from Vipps
├── go.mod
├── go.sum
├── main.go
```

## Environment variables

This project is intended to live inside a Docker Container, therefor i will load the Environment variables thru docker.
In development environment, this will be inside the docker-compose file in .devcontainer folder for VScode
In production this will be present in the docker-compose file used for spinning up the application

### Needed environment variables

- client_id - Client_id for a sales unit.
- client_secret - Client_secret for a sales unit.
- ocp_apim_subscription_key - Subscription key for a sales unit.
- merchant_serial_number - The unique ID for a sales unit.
- first_date - The date the integration will start from, this is to prevent loading old / already loaded data

## Tech stack

### Backend

This will be my first project in Go, and i believe i will write the whole project with Go to be build with a single docker container.
To run the application both in production and development i will use docker with docker-compose.
There is no need for a database. It seems from the Vipps API documentation that we pull out data on a day-day basis, and when a report for a date is complete it will not change. Therefor for efficency, it could be beneficial to save the previously successfully loaded date, if so i will do this in a file.

### Frontend

There will be no need for a frontend

# Installation guide

## Running the app

For the future

- Get API keys from Vipps

## My development workflow

I like to code with devcontainers in VScode, therefor i have the .devcontainer folder in this repository. To get into developer mode in VScode, update .env in the .devcontainer folder to your needs, and run the devcontainer in VScode.

# Notes

## Fetching data from Vipps

There are two ways to fetch data from Vipps:

- [Complete report for each date](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/#method-1-fetching-a-complete-report-for-each-date) - Pull out a completed repport for a specific date, this will be ready when Vipps has processed all the payments for this date. This will not change when available
- [Feed of data](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/#method-2-continuous-feed-of-data) - This will be a continous stream of data, where you give it the last ID you processed to get a new set of data. This will make the data accessable as soon as it is processed

For my case, i want to run the integration on a daily basis, therefor i will use the report feauture as i only need to make one call to the Vipps API.

# TODO

## Setup

- [ ] Sette opp mappe strukturen
- [ ] Lage filer
