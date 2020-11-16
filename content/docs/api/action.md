---
title: Action
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernández"
  - "Seqera Labs"
headline: 'Action'
description: 'API Action'
menu:
  docs:
    parent: API
    weight: 2
    
------------------------------------------------------------------------------------------------

Pipeline actions allow you to launch the execution of your pipeline when submitting a change to the associated code repository


## List actions
List the available Pipeline actions for the authenticated user.

{{% tip %}}
**GET** /actions
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/actions'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "actions": [
        {
            "id": "2jaihu35K9KfdITSC1FrJZ",
            "name": "Sample Action",
            "pipeline": "https://github.com/nf-core/rnafusion",
            "source": "tower",
            "status": "ACTIVE",
            "lastSeen": null,
            "dateCreated": "2020-11-11T07:12:48Z",
            "event": null,
            "usageCmd": "curl -H \"Content-Type: application/json\" \\\n     -H \"Authorization: Bearer <YOUR TOKEN>\" \\\n     https://staging-tower.xyz/api/actions/2jaihu35K9KfdITSC1FrJZ/launch \\\n     --data '{\"params\":{\"foo\":\"Hello world\"}}'",
            "endpoint": "https://staging-tower.xyz/api/actions/2jaihu35K9KfdITSC1FrJZ/launch"
        }
    ]
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

------------------------------------------------------------------------------------------------

## List action types
List the supported event types that can trigger a pipeline action.

{{% tip %}}
**GET** /actions/types
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/actions/types'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "eventTypes": [
        {
            "source": "github",
            "display": "Github web hook",
            "description": "Allow the pipeline execution when pushing a change to the corresponding project repository",
            "enabled": false
        },
        {
            "source": "tower",
            "display": "Tower launch hook",
            "description": "Allow the pipeline execution sending a http request to Tower endpoint",
            "enabled": true
        }
    ]
}
{{< /highlight >}}

### Bad request 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

------------------------------------------------------------------------------------------------

## Describe action
Describe an existing pipeline action.

{{% tip %}}
**GET** /actions/{actionId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `actionId` | `string` | query | **Required**. Action id. |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/actions/{actionId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "action": {
        "id": "4ITo9XoID4pJ8c27VoKrmT",
        "launch": {
            "id": "1uNiTdzg52bf88h4sWjlew",
            "computeEnv": {
                "id": "vTaRly1g9tfe03P7Z9wWb",
                "name": "AWS",
                "description": "Default compute environment",
                "platform": "aws-batch",
                "config": {
                    "region": "eu-west-1",
                    "computeQueue": "TowerForge-vTaRly1g9tfe03P7Z9wWb-work",
                    "computeJobRole": null,
                    "headQueue": "TowerForge-vTaRly1g9tfe03P7Z9wWb-head",
                    "headJobRole": null,
                    "cliPath": "/home/ec2-user/miniconda/bin/aws",
                    "volumes": [],
                    "workDir": "s3://random-tower-bucket",
                    "preRunScript": null,
                    "postRunScript": null,
                    "forge": {
                        "type": "SPOT",
                        "minCpus": 0,
                        "maxCpus": 1,
                        "gpuEnabled": false,
                        "ebsAutoScale": true,
                        "instanceTypes": [],
                        "allocStrategy": null,
                        "imageId": null,
                        "vpcId": null,
                        "subnets": [],
                        "securityGroups": [],
                        "fsxMount": null,
                        "fsxName": null,
                        "fsxSize": null,
                        "disposeOnDeletion": true,
                        "ec2KeyPair": null,
                        "allowBuckets": [
                            "s3://random-tower-bucket"
                        ],
                        "ebsBlockSize": null
                    },
                    "forgedResources": [
                        {
                            "IamRole": "arn:aws:iam::718519895578:role/TowerForge-vTaRly1g9tfe03P7Z9wWb-ServiceRole"
                        },
                        {
                            "IamRole": "arn:aws:iam::718519895578:role/TowerForge-vTaRly1g9tfe03P7Z9wWb-FleetRole"
                        },
                        {
                            "IamInstanceProfile": "arn:aws:iam::718519895578:instance-profile/TowerForge-vTaRly1g9tfe03P7Z9wWb-InstanceRole"
                        },
                        {
                            "Ec2LaunchTemplate": "TowerForge-vTaRly1g9tfe03P7Z9wWb"
                        },
                        {
                            "BatchEnv": "arn:aws:batch:eu-west-1:718519895578:compute-environment/TowerForge-vTaRly1g9tfe03P7Z9wWb-head"
                        },
                        {
                            "BatchQueue": "arn:aws:batch:eu-west-1:718519895578:job-queue/TowerForge-vTaRly1g9tfe03P7Z9wWb-head"
                        },
                        {
                            "BatchEnv": "arn:aws:batch:eu-west-1:718519895578:compute-environment/TowerForge-vTaRly1g9tfe03P7Z9wWb-work"
                        },
                        {
                            "BatchQueue": "arn:aws:batch:eu-west-1:718519895578:job-queue/TowerForge-vTaRly1g9tfe03P7Z9wWb-work"
                        }
                    ]
                },
                "dateCreated": "2020-11-10T15:57:50Z",
                "lastUpdated": "2020-11-10T15:58:26Z",
                "lastUsed": null,
                "deleted": null,
                "status": "AVAILABLE",
                "message": null,
                "primary": true,
                "credentialsId": "68gQz4mHN6wMBbAnPeliG3"
            },
            "pipeline": "https://github.com/nf-core/rnafusion",
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
            "sessionId": "J8c27VoKrmT",
            "configProfiles": [
                "debug",
                "docker"
            ],
            "dateCreated": "2020-11-11T07:34:22Z",
            "lastUpdated": "2020-11-11T07:34:22Z"
        },
        "user": {
            "id": 203,
            "userName": "testUser",
            "email": "sequera@seqera.io",
            "firstName": "John",
            "lastName": "Smith",
            "organization": "Seqera Labs",
            "description": "Test user for Seqera Labs",
            "avatar": "https://www.nextflow.io/",
            "trusted": true,
            "notification": true,
            "options": null,
            "lastAccess": "2020-11-11T06:26:42Z",
            "dateCreated": "2020-11-10T09:54:41Z",
            "lastUpdated": "2020-11-11T07:18:58Z"
        },
        "name": "RNA Fusion action",
        "hookId": "4ITo9XoID4pJ8c27VoKrmT",
        "hookUrl": "https://staging-tower.xyz/api/actions/4ITo9XoID4pJ8c27VoKrmT/launch",
        "message": null,
        "deleted": null,
        "source": "tower",
        "status": "ACTIVE",
        "config": {},
        "event": null,
        "lastSeen": null,
        "dateCreated": "2020-11-11T07:34:22Z",
        "lastUpdated": "2020-11-11T07:34:22Z"
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

## Update action
Update a pipeline action.

{{% tip %}}
**PUT** /actions/{actionId}
{{% /tip %}}

### Request parameters
 Name  | Type     | In | Description             |
|------|----------|----|-------------------------|
| `id` | `string` | body | **Required**. Action id. |
| `name` | `string` | body | **Required**. A descriptive name to identify this action. |
| `source` | `string` | body | **Required**. Action event source (github, tower). |
| `computeEnvId` | `string` | body | **Required**. Compute environment id where the pipeline will run. |
| `pipeline` | `string` | body | **Required**. Pipeline name or a Git repository URL. |
| `workDir` | `string` | body | The bucket path where the pipeline scratch data is stored. |
| `revision` | `string` | body | A valid repository commit id, tag or branch name. |
| `sessionId` | `string` | query | Arbitrary tracking session id. |
| `configProfiles` | `string` | body | Configuration profile names.  |
| `configText` | `string` | body | Extra configuration options. |
| `paramsText` | `string` | body | Additional parameter options. |
| `preRunScript` | `string` | body | Bash script executed before the pipeline is launched. |
| `postRunScript` | `string` | body | Bash script executed after the pipeline is launched. |
| `mainScript` | `string` | body | Pipeline main script file if different from `main.nf` |
| `entryName` | `string` | body | Main workflow name to be executed when using DLS2 syntax. |
| `resume` | `boolean` | body | Resume on fail. |
| `pullLatest` | `boolean` | body | Pulls the latest version from the Git repository before running the pipeline. |

### Sample payload
{{< highlight json >}}
{
    "id": "4ITo9XoID4pJ8c27VoKrmT",
    "name": "RNA Fusion action",
    "source": "tower",
    "launch": {
        "computeEnvId": "vTaRly1g9tfe03P7Z9wWb",
        "pipeline": "https://github.com/nf-core/rnafusion",
        "workDir": "s3://random-tower-bucket",
        "revision": "master",
        "sessionId": "J8c27VoKrmT",
        "configProfiles": [
            "debug",
            "docker"
        ],
        "configText": "{\"config\":{\"config1\":\"value\", \"config2\":\"value\"}}",
        "paramsText": "{\"params\":{\"param1\":\"value\", \"param2\":\"value\"}}",
        "preRunScript": "./runMeBefore.sh",
        "postRunScript": "./runMeAfter.sh",
        "mainScript": "start.nf",
        "entryName": "main_workflow",
        "resume": false,
        "pullLatest": false
    }
}
{{< /highlight >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X PUT 'https://api.tower.nf/api/actions/{actionId}' \
     -d '{"key":"value"}'
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

------------------------------------------------------------------------------------------------

## Create action
Create a new pipeline action.

{{% tip %}}
**POST** /actions
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `name` | `string` | body | **Required**. A descriptive name to identify this action. |
| `source` | `string` | body | **Required**. Action event source (github, tower). |
| `computeEnvId` | `string` | body | **Required**. Compute environment id where the pipeline will run. |
| `pipeline` | `string` | body | **Required**. Pipeline name or a Git repository URL. |
| `workDir` | `string` | body | The bucket path where the pipeline scratch data is stored. |
| `revision` | `string` | body | A valid repository commit id, tag or branch name. |
| `sessionId` | `string` | body | Given session identifier. |
| `configProfiles` | `string` | body | Configuration profile names.  |
| `configText` | `string` | body | Extra configuration options. |
| `paramsText` | `string` | body | Additional parameter options. |
| `preRunScript` | `string` | body | Bash script executed before the pipeline is launched. |
| `postRunScript` | `string` | body | Bash script executed after the pipeline is launched. |
| `mainScript` | `string` | body | Pipeline main script file if different from `main.nf` |
| `entryName` | `string` | body | Main workflow name to be executed when using DLS2 syntax. |
| `resume` | `boolean` | body | Resume on fail. |
| `pullLatest` | `boolean` | body | Pulls the latest version from the Git repository before running the pipeline. |

### Sample payload
{{< highlight json >}}
{
  "name": "RNA Fusion action",
  "source": "tower",
  "launch": {
    "computeEnvId": "vTaRly1g9tfe03P7Z9wWb",
    "pipeline": "https://github.com/nf-core/rnafusion",
    "workDir": "s3://random-tower-bucket",
    "revision": "master",
    "sessionId": "J8c27VoKrmT",
    "configProfiles": [
      "debug",
      "docker"
    ],
    "configText": "{\"config\":{\"config1\":\"value\", \"config2\":\"value\"}}",
    "paramsText": "{\"params\":{\"param1\":\"value\", \"param2\":\"value\"}}",
    "preRunScript": "./runMeBefore.sh",
    "postRunScript": "./runMeAfter.sh",
    "mainScript": "start.nf",
    "entryName": "main_workflow",
    "resume": false,
    "pullLatest": false
  }
}
{{< /highlight >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/actions'
     -d '{"key":"value"}'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "actionId": "71oM1VpCtrRSzzHKVPoAzS"
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

## Pause action
Toggle the pause status of an existing pipeline action.

{{% tip %}}
**POST** /actions/{actionId}/pause
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `actionId` | `string` | query | **Required**. Action id. |

### Code Sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/actions/{actionId}/pause'
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

------------------------------------------------------------------------------------------------

## Execute action
Trigger the execution of a Tower Launch action.

{{% tip %}}
**POST** /actions/{actionId}/launch
{{% /tip %}}

### Request parameters

| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `actionId` | `string` | query | **Required**. Action id. |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/actions/{actionId}/launch'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "workflowId": "14EsC9QBjPEYw0"
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

## Delete action
Delete a pipeline action.

{{% tip %}}
**DELETE** /actions/{actionId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `actionId` | `string` | query | **Required**. Action id. |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X DELETE 'https://api.tower.nf/api/actions/{actionId}'
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


