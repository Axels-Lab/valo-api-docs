# API Reference Overview

Welcome to the API Reference for Valo-API. This section provides detailed information about the available endpoints, request parameters, and response formats.

## Base URL

The base URL for the Valo-API is:

```plaintext
https://valo-api.vercel.app
```

## Endpoints

The Valo-API provides the following endpoints:

- [`/rank`](./rank.md/#rank-endpoint)
- [`/checkrank`](./rank.md/#checkrank-endpoint)
- [`/leaderboard-rank`](./rank.md/#leaderboard-rank-endpoint)
- [`/stats`](./stats.md/#stats-endpoint)
- [`/checkstats`](./stats.md/#checkstats-endpoint)
- [`/record`](./record.md/#record-endpoint)
- [`/last-match/party-members`](./stats.md/#party-members-endpoint)

## Request Parameters

Each endpoint may require specific parameters. Refer to the endpoint documentation for detailed information on required and optional parameters.

## Response Formats

Responses are typically in plain string format to be shown in twitch chat. If you're encountering an issue with 401 Unauthorized, please refer to the [Authentication](../index.md/#authentication) section and make sure you're using a valid API key.

For more detailed information, please refer to the specific endpoint documentation.