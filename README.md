Here's the README documentation for your code:

---

# OAuth2 Client ID and Secret Management Service

This project is built with **Express.js** and **Zoho Catalyst SDK** to manage OAuth2 client credentials, generate access tokens, and handle authentication with Zoho's OAuth2 API.

## Table of Contents
- [Features](#features)
- [Endpoints](#endpoints)
  - [Create Client ID Table](#post-create-client-id-table)
  - [Generate OAuth Request URL](#get-auth-request)
  - [OAuth Callback Handler](#get-invoke-accept)
  - [Fetch Client Details](#get-client-details)
  - [Fetch Token Details](#get-token-details)
- [Environment Setup](#environment-setup)
- [Error Handling](#error-handling)

## Features
- Store OAuth2 client credentials (`client_id` and `client_secret`) for customers.
- Generate an OAuth2 authorization URL.
- Handle OAuth2 callback to exchange authorization code for access and refresh tokens.
- Store access and refresh tokens in Catalyst datastore tables.
- Retrieve client and token details.

## Endpoints

### POST `/create-client-id-table`
This endpoint creates an entry in the datastore to store the `client_id`, `client_secret`, and `customer_id`.

#### Request Body:
```json
{
  "CLIENT_ID": "your-client-id",
  "CLIENT_SECRET": "your-client-secret",
  "CUSTOMER_ID": "your-customer-id"
}
```

#### Responses:
- **200 OK**: When the client ID and secret are successfully stored.
- **409 Conflict**: When the client ID already exists.
- **500 Internal Server Error**: If there's an issue with the Catalyst datastore.

### GET `/auth-request`
Generates an OAuth2 authorization URL with the stored `client_id`. The user is redirected to this URL to grant permission to the application.

#### Response:
```json
{
  "url": "https://accounts.zoho.in/oauth/v2/auth?...&client_id=your-client-id"
}
```

#### Responses:
- **201 Created**: When the URL is successfully generated.
- **404 Not Found**: If no `client_id` exists in the datastore.
- **500 Internal Server Error**: If there is an issue fetching the `client_id`.

### GET `/invoke-accept`
This is the callback endpoint for Zoho OAuth2. After the user grants permission, Zoho redirects the user back to this URL with an authorization code. This code is then exchanged for an access token and a refresh token, which are stored in the datastore.

#### Query Parameters:
- `code`: Authorization code received from Zoho.

#### Responses:
- **200 OK**: When the access and refresh tokens are successfully saved.
- **500 Internal Server Error**: If there is an issue while generating tokens or saving them to the datastore.

### GET `/client-details`
Fetches the stored `client_id`, `client_secret`, and `customer_id`.

#### Response:
```json
{
  "clientDetails": {
    "clientID": "your-client-id",
    "client_secret": "your-client-secret",
    "customerID": "your-customer-id"
  }
}
```

#### Responses:
- **200 OK**: When client details are successfully fetched.
- **404 Not Found**: If no client details exist in the datastore.
- **500 Internal Server Error**: If there is an issue fetching the data.

### GET `/token-details`
Fetches the stored access token and refresh token.

#### Response:
```json
{
  "tokenDetails": {
    "clientID": "your-client-id",
    "access_token": "your-access-token",
    "refresh_token": "your-refresh-token"
  }
}
```

#### Responses:
- **200 OK**: When token details are successfully fetched.
- **404 Not Found**: If no token details exist in the datastore.
- **500 Internal Server Error**: If there is an issue fetching the data.

## Environment Setup

1. **Catalyst Configuration**: 
   - Ensure you have a Zoho Catalyst account.
   - Set up the necessary datastore tables:
     - `ClientId_and_secret_customerId` to store client credentials.
     - `auth_token_refresh_token` to store tokens.
   - Use Catalyst SDK to initialize the app within your Catalyst project.

2. **OAuth2 Redirect URI**:
   - Replace the redirect URI in `/auth-request` and `/invoke-accept` with your app's correct endpoint.

## Error Handling

- The app returns appropriate HTTP status codes:
  - `200 OK` for successful requests.
  - `404 Not Found` if no data is found.
  - `409 Conflict` for client ID duplication.
  - `500 Internal Server Error` for unexpected errors.

## License
This project is licensed under the MIT License.

--- 

This documentation provides a comprehensive overview of your API, its endpoints, and the processes involved. Let me know if you'd like to add or change any sections!

