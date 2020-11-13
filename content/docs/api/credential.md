---
title: Credential
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: 'Credential'
description: 'API Credential'
menu:
  docs:
    parent: API
    weight: 2

------------------------------------------------------------------------------------------------

## List credentials
List available credentials for the user and platform.

{{% tip %}}
**GET** /api/credentials
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/credentials'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": [
        {
            "id": "1mO5qKOUcacMaFXJsmJzQh",
            "name": "Git Hub Access Credentials",
            "description": "Git Hub corporate account",
            "provider": "github",
            "category": "repositories",
            "deleted": null,
            "lastUsed": "2020-11-10T12:20:18.024019Z",
            "dateCreated": "2020-11-10T11:20:18.024019Z",
            "lastUpdated": "2020-11-10T11:20:18.024019Z"
        }
    ]
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## Describe credential
Describe the credentials for a given id.

{{% tip %}}
**GET** /api/credentials/:credentialId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `credentialId` | `string` | query | **Required**. Credential entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/credentials/{credentialId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "Git Hub Access Credentials",
        "description": "Git Hub corporate account",
        "provider": "github",
        "category": "repositories",
        "deleted": null,
        "lastUsed": "2020-11-10T12:20:18.024019Z",
        "dateCreated": "2020-11-10T11:20:18.024019Z",
        "lastUpdated": "2020-11-10T11:20:18.024019Z"
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

## Update credential
Update a credential data for a given id

{{% tip %}}
**PUT** /api/credentials/:credentialId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `id` | `string` | query | **Required**. Credential entity id |
| `name ` | `string` | body | **Required**. Name for the credential entity. |
| `description ` | `string` | body | Long description for the credential entity id. |
| `provider ` | `string` | body | **Required**. Credential provider (github, gitlab, bitbucket, google, aws, ssh). |
| `category ` | `string` | body | Category description. |
| `keys ` | `object` | body | **Required**. Set of access and secret keys from provider. |

### Sample payload 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "Git Hub Access Credentials",
        "description": "Git Hub corporate account",
        "provider": "github",
        "category": "repositories",
         "keys": {
              "accessKey": "FKIC2OSY9QYNOGDPTS6R",
              "secretKey": "LBf5hPpuDy/jH02JCt7lzH7jDtcBHMEiyoHPhzvz"
         }
    }
}
{{< /highlight >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X PUT 'https://api.tower.nf/api/credentials/{credentialId}'
{{< /highlight >}}

### Default response 
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

## Create credential
Create a new credential data.

{{% tip %}}
**POST** /api/credentials
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `name ` | `string` | body | **Required**. Name for the credential entity. |
| `description ` | `string` | body | Long description for the credential entity id. |
| `provider ` | `string` | body | **Required**. Credential provider (github, gitlab, bitbucket, google, aws, ssh). |
| `category ` | `string` | body | Category description. |
| `keys ` | `object` | body | **Required**. Set of access and secret keys from provider. |

### Sample payload 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "Git Hub Access Credentials",
        "description": "Git Hub corporate account",
        "provider": "github",
        "category": "repositories",
         "keys": {
              "accessKey": "FKIC2OSY9QYNOGDPTS6R",
              "secretKey": "LBf5hPpuDy/jH02JCt7lzH7jDtcBHMEiyoHPhzvz"
            }
    }
}
{{< /highlight >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/credentials'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentialsId": "1mO5qKOUcacMaFXJsmJzQh"
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

## Delete credential
Delete the credential record for the given id.

{{% tip %}}
**DELETE** /api/credentials/:credentialId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `credentialId` | `string` | query | **Required**. Credential entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X DELETE 'https://api.tower.nf/api/credentials/{credentialId}'
{{< /highlight >}}

### Default response 
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
