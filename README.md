# DAZN code challenge

## Assumptions

This is a microservice with only responsible for providing other services with the information about whether the user requested is currently consuming 3 streams or more.

The storage service will be updated by another service (maybe via websockets?)
The service is not probably public facing, so no authentication/authorisation was provided, the assumption is the `streamService` responsible for serving the videos will be the only consumer

## Running the app locally

Checkout the project in GitHub, then run the following commands in the root directory:

```
./scripts/docker-run-locally.sh
```

## Running tests

```
nvm use
npm run test
```
or in a CI environment

```
./scripts/docker-run-unit-tests.sh
```

## Architecture

- 1 (any) node js stateless service(s)
- 1 dynamoDB database
- Logging and metrics: Datadog

## Endpoints

- The only available endpoint is `/user/:userID` which returns:
    - `200` if the has streams
    - `200` if there are not streams for the user provided
    - `404` if no user is provided
    - `429` if user is watching 3 or more streams
    - `502` if connection with the `storage service` fails

- Valid user: http://localhost:8000/user/1
- Quota exceeded user: http://localhost:8000/user/2
- Non existing user: http://localhost:8000/user/3

## Deployment

- A `Dockerfile` is provided which could be deployed using `ECS` or any other service
- A `dist` folder is provided by running `npm run build` containing a full node + express app.

For the application to work the following env variables MUST be provided (see more in `./src/infra/config.ts`):

```
      - ENV: Enviroment for the app (local, prod, ...)
      - LOG_LEVEL: The desired log level
      - PORT: The port where the app will run
      - STATSD_HOST: The host for the metrics service
      - STATSD_PORT: The port for the metrics service
      - AWS_ACCESS_KEY_ID: Access key for AWS
      - AWS_SECRET_ACCESS_KEY: Secrete key for AWS
      - AWS_REGION: Region for AWS
```

In addition for local development to mock `DynamoDB` the env variable `AWS_DYNAMODB_ENDPOINT` could be provided.