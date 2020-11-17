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
**GET** /api/credentials/{credentialId}
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
**PUT** /api/credentials/{credentialId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `name ` | `string` | body | **Required**. Name for the credential entity. |
| `description ` | `string` | body | Long description for the credential entity id. |
| `provider ` | `string` | body | **Required**. Credential provider (github, gitlab, bitbucket, google, aws, ssh). |
| `category ` | `string` | body | Category description. |
| `keys ` | `object` | body | **Required when requested**. Set of access and secret keys from provider. |
| `data ` | `object` | body | **Required when requested**. Provider API access json credentials. |
| `username ` | `object` | body | **Required when requested**. Account username. |
| `password ` | `object` | body | **Required when requested**. Account password. |
| `access_token ` | `object` | body | **Required when requested**. Account access token. |
| `privateKey ` | `object` | body | **Required when requested**. Private SSH key. |
| `passphrase ` | `object` | body | Private SSH key passphrase when necessary. |

{{< tip >}}
All credentials are securely stored using advanced encryption (AES-256) and never exposed by any Tower API.
{{< /tip >}}

### Sample payload for AWS
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "AWS Access Credentials",
        "description": "Some AWS corporate account",
        "provider": "aws",
        "category": "accounts",
        "keys": {
              "accessKey": "FKIC2OSY9QYNOGDPTS6R",
              "secretKey": "LBf5hPpuDy/jH02JCt7lzH7jDtcBHMEiyoHPhzvz"
            }
    }
}
{{< /highlight >}}

{{< tip >}}
Make sure the AWS user account includes the IAM permissions required by [Tower Launch](https://github.com/seqeralabs/nf-tower-aws/tree/master/launch) 
and [Forge](https://github.com/seqeralabs/nf-tower-aws/tree/master/forge).
{{< /tip >}}

### Sample payload for Google
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "Google Access Credentials",
        "description": "Some Google corporate account",
        "provider": "google",
        "category": "repositories",
        "data" : {        	
            "type":	"service_account",
            "project_id": "project-id",
            "private_key_id": "6d1kid834937a945de13ab23f94395art8d239107",
            "private_key": "-----BEGIN PRIVATE KEY-----\n
                        MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQC5UYLh2ISurLek\n
                        MpjsKKGGBTwC14q5SuBnJrbk5MLbaupH/6szE1ikO8be0MTYxBxhYoIi8LvmMpqA\n
                        xyH0BaKE5K8xAN9XeXS/vTz6g83tZoBorL3RmkjNRKLFfzwX8L+OIxun6Bvv426Q\n                    
                        -----END PRIVATE KEY-----\n",
            "client_email":	"project-account@project-id.iam.gserviceaccount.com",
            "client_id": "911649436297717894125",
            "auth_uri": "https://accounts.google.com/o/oauth2/auth",
            "token_uri": "https://oauth2.googleapis.com/token",
            "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
            "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/project-id%project-id.iam.gserviceaccount.com"
        }
    }
}
{{< /highlight >}}

{{< tip >}}
Refer to the [Google documentation](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) for 
details about service account configuration.
{{< /tip >}}

### Sample payload for Bitbucket
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "Bitbucket Hub Access Credentials",
        "description": "Bitbucket corporate account",
        "provider": "bitbucket",
        "username": "username@some-email.com",
        "password": "xyH0BaKE5K8"
    }
}
{{< /highlight >}}

{{< tip >}}
App passwords are substitute passwords for a user account which you can use for scripts and integrating tools to avoid putting your real password into configuration files.
Learn how to create a BitBucket App password at [this link](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/).
{{< /tip >}}

### Sample payload for Github
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "Git Hub Access Credentials",
        "description": "Some Git Hub corporate account",
        "provider": "github",
        "username": "username@some-email.com",
        "password": "xyH0BaKE5K8"
    }
}
{{< /highlight >}}

{{< tip >}}
Refer to the [GitHub](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token) 
documentation on how to create a GitHub access token as a replacement for a password.
{{< /tip >}}

### Sample payload for Git Lab
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "Git Lab Access Credentials",
        "description": "Git Lab corporate account",
        "provider": "gitlab",
        "username": "username@some-email.com",
        "access_token" : "LBf5hPpuDy/jH02JCt7lzH7jDtcBHMEiyoHPhzvz"
    }
}
{{< /highlight >}}

{{< tip >}}
The GitLab API access token can be found in your [GitLab account page](https://gitlab.com/profile/personal_access_tokens)
 (sign in required). Make sure to include the following scopes: `api`, `read_api`, `read_repository`.
{{< /tip >}}

### Sample payload for SSH
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "id": "1mO5qKOUcacMaFXJsmJzQh",
        "name": "SSH Access Credentials",
        "description": "Remote SSH access",
        "provider": "ssh",
        "privateKey": "-----BEGIN RSA PRIVATE KEY-----\n
                       MIICXAIBAAKBgQCqGKukO1De7zhZj6+H0qtjTkVxwTCpvKe4eCZ0FPqri0cb2JZfXJ/DgYSF6vUp\n
                       wmJG8wVQZKjeGcjDOL5UlsuusFncCzWBQ7RKNUSesmQRMSGkVb1/3j+skZ6UtW+5u09lHNsj6tQ5\n
                       1s1SPrCBkedbNf0Tp0GbMJDyR4e9T04ZZwIDAQABAoGAFijko56+qGyN8M0RVyaRAXz++xTqHBLh\n
                       3tx4VgMtrQ+WEgCjhoTwo23KMBAuJGSYnRmoBZM3lMfTKevIkAidPExvYCdm5dYq3XToLkkLv5L2\n
                       -----END RSA PRIVATE KEY-----",
        "passphrase": "LBf5hPpuDy"
    }
}
{{< /highlight >}}

{{< tip >}}
Specify the content of the private key file from the SSH asymmetrical key pair to enable SSH connection to the target environment. A new SSH key pair can be created using the command: ssh-keygen.
{{< /tip >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X PUT 'https://api.tower.nf/api/credentials/{credentialId}'
     -d '{"credentials":{"key":"value"}}'
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
| `keys ` | `object` | body | **Required when requested**. Set of access and secret keys from provider. |
| `data ` | `object` | body | **Required when requested**. Provider API access json credentials. |
| `username ` | `object` | body | **Required when requested**. Account username. |
| `password ` | `object` | body | **Required when requested**. Account password. |
| `access_token ` | `object` | body | **Required when requested**. Account access token. |
| `privateKey ` | `object` | body | **Required when requested**. Private SSH key. |
| `passphrase ` | `object` | body | Private SSH key passphrase when necessary. |

{{< tip >}}
All credentials are securely stored using advanced encryption (AES-256) and never exposed by any Tower API.
{{< /tip >}}

### Sample payload for AWS
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "AWS Access Credentials",
        "description": "Some AWS corporate account",
        "provider": "aws",
        "category": "accounts",
        "keys": {
              "accessKey": "FKIC2OSY9QYNOGDPTS6R",
              "secretKey": "LBf5hPpuDy/jH02JCt7lzH7jDtcBHMEiyoHPhzvz"
            }
    }
}
{{< /highlight >}}

{{< tip >}}
Make sure the AWS user account includes the IAM permissions required by [Tower Launch](https://github.com/seqeralabs/nf-tower-aws/tree/master/launch) 
and [Forge](https://github.com/seqeralabs/nf-tower-aws/tree/master/forge).
{{< /tip >}}

### Sample payload for Google
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "Google Access Credentials",
        "description": "Some Google corporate account",
        "provider": "google",
        "category": "repositories",
        "data" : {        	
            "type":	"service_account",
            "project_id": "project-id",
            "private_key_id": "6d1kid834937a945de13ab23f94395art8d239107",
            "private_key": "-----BEGIN PRIVATE KEY-----\n
                        MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQC5UYLh2ISurLek\n
                        MpjsKKGGBTwC14q5SuBnJrbk5MLbaupH/6szE1ikO8be0MTYxBxhYoIi8LvmMpqA\n
                        xyH0BaKE5K8xAN9XeXS/vTz6g83tZoBorL3RmkjNRKLFfzwX8L+OIxun6Bvv426Q\n                    
                        -----END PRIVATE KEY-----\n",
            "client_email":	"project-account@project-id.iam.gserviceaccount.com",
            "client_id": "911649436297717894125",
            "auth_uri": "https://accounts.google.com/o/oauth2/auth",
            "token_uri": "https://oauth2.googleapis.com/token",
            "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
            "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/project-id%project-id.iam.gserviceaccount.com"
        }
    }
}
{{< /highlight >}}

{{< tip >}}
Refer to the [Google documentation](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) for 
details about service account configuration.
{{< /tip >}}

### Sample payload for Bitbucket
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "Bitbucket Hub Access Credentials",
        "description": "Bitbucket corporate account",
        "provider": "bitbucket",
        "username": "username@some-email.com",
        "password": "xyH0BaKE5K8"
    }
}
{{< /highlight >}}

{{< tip >}}
App passwords are substitute passwords for a user account which you can use for scripts and integrating tools to avoid putting your real password into configuration files.
Learn how to create a BitBucket App password at [this link](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/).
{{< /tip >}}

### Sample payload for Github
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "Git Hub Access Credentials",
        "description": "Some Git Hub corporate account",
        "provider": "github",
        "username": "username@some-email.com",
        "password": "xyH0BaKE5K8"
    }
}
{{< /highlight >}}

{{< tip >}}
Refer to the [GitHub](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token) 
documentation on how to create a GitHub access token as a replacement for a password.
{{< /tip >}}

### Sample payload for Git Lab
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "Git Lab Access Credentials",
        "description": "Git Lab corporate account",
        "provider": "gitlab",
        "username": "username@some-email.com",
        "access_token" : "LBf5hPpuDy/jH02JCt7lzH7jDtcBHMEiyoHPhzvz"
    }
}
{{< /highlight >}}

{{< tip >}}
The GitLab API access token can be found in your [GitLab account page](https://gitlab.com/profile/personal_access_tokens)
 (sign in required). Make sure to include the following scopes: `api`, `read_api`, `read_repository`.
{{< /tip >}}

### Sample payload for SSH
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "credentials": {
        "name": "SSH Access Credentials",
        "description": "Remote SSH access",
        "provider": "ssh",
        "privateKey": "-----BEGIN RSA PRIVATE KEY-----\n
                       MIICXAIBAAKBgQCqGKukO1De7zhZj6+H0qtjTkVxwTCpvKe4eCZ0FPqri0cb2JZfXJ/DgYSF6vUp\n
                       wmJG8wVQZKjeGcjDOL5UlsuusFncCzWBQ7RKNUSesmQRMSGkVb1/3j+skZ6UtW+5u09lHNsj6tQ5\n
                       1s1SPrCBkedbNf0Tp0GbMJDyR4e9T04ZZwIDAQABAoGAFijko56+qGyN8M0RVyaRAXz++xTqHBLh\n
                       3tx4VgMtrQ+WEgCjhoTwo23KMBAuJGSYnRmoBZM3lMfTKevIkAidPExvYCdm5dYq3XToLkkLv5L2\n
                       -----END RSA PRIVATE KEY-----",
        "passphrase": "LBf5hPpuDy"
    }
}
{{< /highlight >}}

{{< tip >}}
Specify the content of the private key file from the SSH asymmetrical key pair to enable SSH connection to the target environment. A new SSH key pair can be created using the command: ssh-keygen.
{{< /tip >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/credentials'
     -d '{"credentials":{"key":"value"}}'
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
**DELETE** /api/credentials/{credentialId}
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


[]: https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/
