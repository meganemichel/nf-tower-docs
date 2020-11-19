---
title: Overview
aliases:
- "/docs/api"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: "Overview"
description: 'Using the Nextflow Tower API.'
menu:
  docs:
    parent: API
    weight: 1


---

Tower is API centric, it exposes a public API with all necessary calls to manage and monitor Nextflow workflows programmatically. This allows organizations to extend their existing solutions by leveraging the Tower API.

## Current version
All requests to https://api.tower.nf receive the v1 version of the REST API. To specify a different existing API 
version, you may use the `Accept-Version` or `X-API-VERSION` header.

Use the https://api.tower.nf/service-info entry point for current system accurate information.

## Schema
All API access is over HTTPS, and accessed from https://api.tower.nf. All data is sent and received as JSON.

All timestamps sent and returned are in ISO 8601 standard format:

{{< tip >}}
YYYY-MM-DDTHH:MM:SSZ
{{< /tip >}}


## Authentication

Authorization tokens can be found in your settings, at the top-right corner of the page under the "Your tokens" section.

{{% pretty_screenshot img="/uploads/2020/11/your_tokens.png" %}}

To create a new access token, just provide a name for the token. This will help to identify the token later.

{{% pretty_screenshot img="/uploads/2020/11/token_form.png" %}}

Once created, tokens can only be seen once when they are initially created. It is important you keep this token at a safe place.

{{% pretty_screenshot img="/uploads/2020/11/personal_access_token.png" %}}

Once created, use the token to authenticate via cURL, Postman, or within your code against the Nextflow API to perform the necessary calls for completing your tasks. 
Please remember that, as any other Bearer token, this token must be included in every API call.


### Example call using the cURL command


{{< highlight bash >}}
curl -H "Authorization: Bearer eyJ…YTk=" https://tower.nf/api/workflow
{{< /highlight >}}

where `QH..E5M=` is the authorization token shown in the screenshot above.

{{% note "Use your token in every API call"%}}

Please remember that, as any other Bearer token, this token must be included in every API call.

{{% /note %}}

## Parameters
Some API `GET` methods will accept standard `query` parameters, which are defined in the documentation; `querystring` optional 
parameters such as page size, number (when available) and file name; and body parameters, mostly used for `POST`, `PUT` and `DELETE` requests.

Additionally, several head parameters are accepted such as `Authorization` for bearer access token or `Accept-Version` to indicate the desired API version to use (default to version 1.0.)

{{< highlight bash >}}
curl -H "Authorization: Bearer QH..E5M=" 
     -H "Accept-Version:1.0"
     -X POST https://tower.nf/api/domain/{item_id}?queryString={value}
     -d { params: { "key":"value" } }
{{< /highlight >}}

## Client errors
There exists two typical standard errors, or non `200` or `204` status responses, to expect from the API.

### Bad request 
The request payload is not properly defined or the query parameters are invalid.

{{< highlight json >}}
{
    "message": "Oops... Unable to process request - Error ID: 54apnFENQxbvCr23JaIjLb"
}
{{< /highlight >}}

### Forbidden 
Your access token is invalid or expired. This response may also imply that the entry point you are trying to access is not available; 
in such a case, it is recommended you check your request syntax.

{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## HTTP verbs
The following table describes the standard and API Rest verbs used.

| Verb | Description |
|------|-------------|
| `HEAD` | Can be issued against any resource to get just the HTTP header info |
| `GET` | Used for retrieving resources |
| `POST` | Used for creating resources |
| `PUT` | Used for updating resources |
| `DELETE` | Used for deleting resources |

All verbs are executed against the current authenticated user or access key.

## Rate limiting
For all API requests, there is a threshold of 20 calls per second (72000 calls per hour) and access key. 

## API docs
Detailed API documentation can be found in the [API page](https://tower.nf/openapi/index.html).
