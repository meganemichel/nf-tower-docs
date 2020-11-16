---
title: Token
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: 'Token'
description: 'API Token'
menu:
  docs:
    parent: API
    weight: 2

------------------------------------------------------------------------------------------------

## List tokens
List the all account token records.

{{% tip %}}
**GET** /api/tokens
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/tokens'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "tokens": [
        {
            "id": 209,
            "token": null,
            "name": "Remote access token",
            "lastUsed": "2020-11-12T11:55:24Z",
            "dateCreated": "2020-11-10T09:56:35Z",
            "basicAuth": null
        }
    ]
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## Create tokens
Create a new token.

{{% tip %}}
**POST** /api/tokens
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `name` | `string` | body | **Required**. Token name. |

### Sample payload
{{< highlight json >}}
{
    "name": "API access token"
}
{{< /highlight >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/tokens'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "accessKey": "eyJ0aWQiOiAyMTJ9LjcyMTlhMGFhNGUxZTk4Zjc1NWIxZmQxNWJlMGZlZjk0MzYzYjQ5ZGM=",
    "token": {
        "id": 212,
        "token": null,
        "name": "SPA Token",
        "lastUsed": null,
        "dateCreated": "2020-11-12T11:59:30.418458Z",
        "basicAuth": null
    }
}
{{< /highlight >}}

### Bad request 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

{{< highlight json >}}
{
    "message": "Oops... Unable to process request - Error ID: 54apnFENQxbvCr23JaIjLb"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## Delete token

Delete a token entity with a given ID

{{% tip %}}
**DELETE** /api/tokens/{tokenId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `tokenId` | `int` | query | **Required**. Token entity id |

### Code Sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X DELETE 'https://api.tower.nf/api/tokens/{tokenId}'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 204 No Content
{{< /highlight >}}

### Bad request 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

{{< highlight json >}}
{
    "message": "Oops... Unable to process request - Error ID: 54apnFENQxbvCr23JaIjLb"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## Delete all tokens

Delete all existing token entities

{{% tip %}}
**DELETE** /api/tokens/delete-all
{{% /tip %}}

### Code Sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X DELETE 'https://api.tower.nf/api/tokens/delete-all'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 204 No Content
{{< /highlight >}}

### Bad request 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

{{< highlight json >}}
{
    "message": "Oops... Unable to process request - Error ID: 54apnFENQxbvCr23JaIjLb"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}
