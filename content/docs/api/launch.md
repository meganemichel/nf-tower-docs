---
title: Launch
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: 'Launch'
description: 'API Launch'
menu:
  docs:
    parent: API
    weight: 2

------------------------------------------------------------------------------------------------

## Describe launch
Describe s launch record for the given id.

{{% tip %}}
**GET** /api/launch/:launchId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `launchId` | `string` | query | **Required**. Launch entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/launch/{launchId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "launch": {
        "id": "EbsBkfTJD9bza31FP4qi6",
        "computeEnv": {
            "id": "6HkkIgaYT7iqwO7V0n6aWa",
            "name": "Local Env",
            "description": null,
            "platform": "local-platform",
            "config": {
                "workDir": null,
                "preRunScript": null,
                "postRunScript": null
            },
            "dateCreated": "2020-11-12T15:50:36.422647+01:00",
            "lastUpdated": "2020-11-12T15:50:36.422647+01:00",
            "lastUsed": "2020-11-13T08:30:02.341447+01:00",
            "deleted": null,
            "status": "AVAILABLE",
            "message": null,
            "primary": true,
            "credentialsId": "7jHplFIeWCBZ2HU825UHXw"
        },
        "pipeline": "https://github.com/nextflow-io/rnaseq-nf",
        "workDir": "s3://random-tower-bucket",
        "revision": "master",
        "configText": "{\"params\":{\"param1\":\"value\", \"param2\":\"value\"}}",
        "paramsText": "{\"params\":{\"param1\":\"value\", \"param2\":\"value\"}}",
        "preRunScript": "./runMeBefore.sh",
        "postRunScript": "./runMeAfter.sh",
        "mainScript": "start.nf",
        "entryName": "main_workflow",
        "resume": false,
        "pullLatest": false,
        "sessionId": "CmMDhg",
        "configProfiles": [
            "docker"
        ],
        "dateCreated": "2020-11-13T08:30:00.940515+01:00",
        "lastUpdated": "2020-11-13T08:30:02.352053+01:00"
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

------------------------------------------------------------------------------------------------
