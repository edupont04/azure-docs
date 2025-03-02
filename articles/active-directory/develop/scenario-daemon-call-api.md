---
title: Call a web API from a daemon app
titleSuffix: Microsoft identity platform 
description: Learn how to build a daemon app that calls a web API.
services: active-directory
author: jmprieur
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev

#Customer intent: As an application developer, I want to know how to write a daemon app that can call web APIs by using the Microsoft identity platform.

---

# Daemon app that calls web APIs - call a web API from the app

.NET daemon apps can call a web API. .NET daemon apps can also call several pre-approved web APIs.

## Calling a web API from a daemon application

Here's how to use the token to call an API:

# [.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# [Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

# [Node.js](#tab/nodejs)

Using an HTTP client like [Axios](https://www.npmjs.com/package/axios), call the API endpoint URI with an access token as the *authorization bearer*.

```JavaScript
const axios = require('axios');

async function callApi(endpoint, accessToken) {

    const options = {
        headers: {
            Authorization: `Bearer ${accessToken}`
        }
    };

    console.log('request made to web API at: ' + new Date().toString());

    try {
        const response = await axios.default.get(endpoint, options);
        return response.data;
    } catch (error) {
        console.log(error)
        return error;
    }
};
```

# [Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

---

## Calling several APIs

For daemon apps, the web APIs that you call need to be pre-approved. There's no incremental consent with daemon apps. (There's no user interaction.) The tenant admin needs to provide consent in advance for the application and all the API permissions. If you want to call several APIs, acquire a token for each resource, each time calling `AcquireTokenForClient`. MSAL will use the application token cache to avoid unnecessary service calls.

## Next steps

# [.NET](#tab/dotnet)

Move on to the next article in this scenario,
[Move to production](./scenario-daemon-production.md?tabs=dotnet).

# [Java](#tab/java)

Move on to the next article in this scenario,
[Move to production](./scenario-daemon-production.md?tabs=java).

# [Node.js](#tab/nodejs)

Move on to the next article in this scenario,
[Move to production](./scenario-daemon-production.md?tabs=nodejs).

# [Python](#tab/python)

Move on to the next article in this scenario,
[Move to production](./scenario-daemon-production.md?tabs=python).

---
