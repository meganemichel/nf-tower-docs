---
title: Compute Environment
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: 'Compute Environments'
description: 'API Compute Environments'
menu:
  docs:
    parent: API
    weight: 2

------------------------------------------------------------------------------------------------

## List compute environments
List all Tower compute environments for the given user.

{{% tip %}}
**GET** /api/compute-envs
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \   
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/compute-envs'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "message": null,
    "computeEnvs": [
        {
            "id": "5DDSatOUG2zd1fyqOBugBv",
            "name": "AWS Default Environment",
            "platform": "aws-batch",
            "status": "AVAILABLE",
            "message": null,
            "lastUsed": null,
            "primary": true
        }
    ]
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

------------------------------------------------------------------------------------------------

## Describe compute environments
Describe a Tower compute environment.

{{% tip %}}
**GET** /api/compute-envs/{computeEnvId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `computeEnvId` | `string` | query | **Required**. Compute environment entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/compute-envs/{computeEnvId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
	"computeEnv": {
		"id": "5SCgzo3x3U2xnIQhO1oJ9o",
		"name": "AWS Default Environment",
		"description": "Principal AWS deployment environment",
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
			"preRunScript": "./preRunScript.sh",
			"postRunScript": "./postRunScript.sh",
			"forge": {
				"type": "EC2",
				"minCpus": 2,
				"maxCpus": 5,
				"gpuEnabled": false,
				"ebsAutoScale": true,
				"instanceTypes": [
					"optimal"
				],
				"allocStrategy": "BEST_FIT",
				"imageId": "string",
				"vpcId": "vpc-a77267c1",
				"subnets": [
					"subnet-a7ff7ffd"
				],
				"securityGroups": [
					"sg-51994d266"
				],
				"fsxMount": null,
				"fsxName": null,
				"fsxSize": null,
				"disposeOnDeletion": false,
				"ec2KeyPair": "key-06a51b08bc3b9c116",
				"allowBuckets": [
					"s3://random-tower-bucket"
				],
				"ebsBlockSize": 50
			},
			"forgedResources": [{
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
		}
	},
	"dateCreated": "2020-11-11T09:37:40Z",
	"lastUpdated": "2020-11-11T09:38:08Z",
	"lastUsed": null,
	"deleted": null,
	"status": "AVAILABLE",
	"message": null,
	"primary": true,
	"credentialsId": "68gQz4mHN6wMBbAnPeliG3"
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

## Create compute environments
Create a new Tower compute environment.

{{% tip %}}
**POST** /api/compute-envs
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `name` | `string` | body | **Required**. Compute environment name. |
| `description` | `string` | body | Compute environment description. |
| `platform` | `string` | body | **Required**. Platform where the execution will be deployed.|
| `credentialsId` | `string` | body | **Required**. Platform credentials id required to access this service. |
| `region` | `string` | body | **Required**. Platform region id where the workload will be deployed. |
| `workDir` | `string` | body | Either an S3 bucket path or a FSx for Lustre directory path. |
| `preRunScript` | `string` | body | Optional Bash script that's executed just before the pipeline is launched. |
| `postRunScript` | `string` | body | Optional Bash script that's executed just after the pipeline is launched. |
| `instanceTypes` | `array` | body | instance types to be used to carry out the computation. |
| `allocStrategy` | `string` | body | Allocation Strategies allow you to choose how Batch launches instances on your behalf. |
| `vpcId` | `string` | body | Container instance network id. |
| `subnets` | `array` | body | One or more subnets in your VPC that can be used to isolate the EC2 resources from each other or from the Internet. |
| `securityGroups` | `array` | body | The security group defines a set of firewall rules that control the traffic for your EC2 compute nodes. |
| `ec2KeyPair` | `array` | body | The EC2 key pair to be installed in the compute nodes to access via SSH. |
| `ebsBlockSize` | `array` | body | Controls the initial size of the EBS auto-expandable volume . |

### Sample payload
{{< highlight json >}}
{
    "computeEnv": {
        "name": "AWS Default Environment",
        "description": "Principal AWS deployment environment",
        "platform": "aws-batch",
        "credentialsId": "68gQz4mHN6wMBbAnPeliG3",
        "config": {
            "region": "eu-west-1",
            "workDir": "s3://random-tower-bucket",
            "forge": {
                "type": "EC2",
                "minCpus": 1,
                "maxCpus": 1,
                "gpuEnabled": false,
                "ebsAutoScale": true,
                "instanceTypes": [
                    "a1"
                ],
                "allocStrategy": "BEST_FIT",            
                "disposeOnDeletion": true,
                "allowBuckets": [
                    "s3://random-tower-bucket"
                ]
            }
        },
        "primary": true
    }
}
{{< /highlight >}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/compute-envs'
     -d '{"key" : "value"}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "computeEnvId": "5SCgzo3x3U2xnIQhO1oJ9o"
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

## Primary compute environments
Defines the primary Tower compute environment.

{{% tip %}}
**POST** /api/compute-envs/{computeEnvId}/primary
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `computeEnvId` | `string` | query | **Required**. Compute environment entity id |


### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \ 
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'https://api.tower.nf/api/compute-envs/{computeEnvId}/primary'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 204 No Content
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

------------------------------------------------------------------------------------------------

## Delete compute environments
Delete an existing Tower compute environment.

{{% tip %}}
**DELETE** /compute-envs/{computeEnvId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `computeEnvId` | `string` | query | **Required**. Compute environment id. |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X DELETE 'https://api.tower.nf/api/compute-envs/{computeEnvId}'
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
