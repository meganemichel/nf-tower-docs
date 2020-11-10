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

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Ut vitae purus nec turpis imperdiet feugiat. Mauris ligula 
nulla, feugiat nec commodo in, fringilla non mi. Nulla molestie orci vehicula quam dictum, eu dapibus dolor 
consectetur. Curabitur luctus sapien ac pulvinar semper. Aenean convallis sagittis felis vitae dapibus.

## Describe launch
Describe the launch record for the given id

{{% tip %}}
**GET** /api/launch/:launchId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `launchId` | `string` | query | **Required**. Compute environment entity id |

### Code sample
{{< highlight bash >}}
curl -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/launch/{launchId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
  "launch": {
    "id": "string",
    "computeEnv": {
      "id": "string",
      "name": "string",
      "description": "string",
      "platform": "string",
      "config": {
        "region": "string",
        "computeQueue": "string",
        "computeJobRole": "string",
        "headQueue": "string",
        "headJobRole": "string",
        "cliPath": "string",
        "volumes": [
          "string"
        ],
        "workDir": "string",
        "preRunScript": "string",
        "postRunScript": "string",
        "forge": {
          "type": "string",
          "minCpus": 0,
          "maxCpus": 0,
          "gpuEnabled": false,
          "ebsAutoScale": false,
          "instanceTypes": [
            "string"
          ],
          "allocStrategy": "string",
          "imageId": "string",
          "vpcId": "string",
          "subnets": [
            "string"
          ],
          "securityGroups": [
            "string"
          ],
          "fsxMount": "string",
          "fsxName": "string",
          "fsxSize": 0,
          "disposeOnDeletion": false,
          "ec2KeyPair": "string",
          "allowBuckets": [
            "string"
          ],
          "ebsBlockSize": 0
        },
        "forgedResources": [
          {}
        ]
      },
      "dateCreated": "1970-01-01T00:00:00.000Z",
      "lastUpdated": "1970-01-01T00:00:00.000Z",
      "lastUsed": "1970-01-01T00:00:00.000Z",
      "deleted": false,
      "status": {},
      "message": "string",
      "primary": false,
      "credentialsId": "string"
    },
    "pipeline": "string",
    "workDir": "string",
    "revision": "string",
    "configText": "string",
    "paramsText": "string",
    "preRunScript": "string",
    "postRunScript": "string",
    "mainScript": "string",
    "entryName": "string",
    "resume": false,
    "pullLatest": false,
    "sessionId": "string",
    "configProfiles": [
      "string"
    ],
    "dateCreated": "1970-01-01T00:00:00.000Z",
    "lastUpdated": "1970-01-01T00:00:00.000Z"
  }
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}
