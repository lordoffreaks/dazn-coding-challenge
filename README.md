# DAZN code challenge

## Running the app locally (live-reloads on file changes)

Checkout the project in GitHub, then run the following commands in the root directory:

```
$ nvm use
$ npm ci
$ npm run dev
```

## Architecture

- 1 (any) node js stateless service(s)
- 1 dynamoDB database
- Logging and metrics: Datadog

## Endpoints

- http://localhost:8000/user/1
