---
title: Platform
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: 'Platform'
description: 'API Platform'
menu:
  docs:
    parent: API
    weight: 2

------------------------------------------------------------------------------------------------

## List platforms 
List available computing platforms.

{{% tip %}}
**GET** /platforms
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/platforms'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "platforms": [
        {
            "id": "slurm-platform",
            "name": "Slurm Workload Manager",
            "credentialsProvider": "ssh"
        },
        {
            "id": "lsf-platform",
            "name": "IBM LSF",
            "credentialsProvider": "ssh"
        },
        {
            "id": "aws-batch",
            "name": "Amazon Batch",
            "credentialsProvider": "aws"
        },
        {
            "id": "google-lifesciences",
            "name": "Google Life Sciences",
            "credentialsProvider": "google"
        }
    ]
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## List platform regions
List available regions for the platform specified.

{{% tip %}}
**GET** /platforms/{platformId}/regions
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `platformId` | `string` | query | **Required**. Platform id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/platforms/{platformId}/regions'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "regions": [
        {
            "id": "us-east-1",
            "name": "US East (N. Virginia)"
        },
        {
            "id": "us-east-2",
            "name": "US East (Ohio)"
        },
        {
            "id": "us-west-1",
            "name": "US West (N. California)"
        }
    ]
}
{{< /highlight >}}

### Bad request 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

{{< highlight json >}}
{
    "message": "Cannot find provider for platform id: unknown-platform-id"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## Describe platform
Describe the platform entity by a given id.

{{% tip %}}
**GET** /platforms/{platformId}?regionId={regionId}&credentialsId={credentialsId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description                |
|------|----------|----|----------------------------|
| `platformId` | `string` | query | **Required**. Platform id |
| `regionId` | `string` | query | **Required**. Platform available region id |
| `credentialsId` | `string` | query | **Required**. Platform valid credentials id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'https://api.tower.nf/api/platforms/{platformId}/?regionId={regionId}&credentialsId={credentialsId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "metainfo": {
        "@class": "io.seqera.tower.service.platform.aws.AwsBatchPlatformMetainfo",
        "warnings": [],
        "jobQueues": [],
        "buckets": [
            {
                "path": "s3://random-tower-bucket"
            }
        ],
        "fileSystems": [],
        "keyPairs": [],
        "vpcs": [
            {
                "id": "vpc-f0979997",
                "isDefault": true
            }
        ],
        "securityGroups": [
            {
                "id": "sg-7b504f07",
                "name": "default",
                "vpcId": "vpc-f0979997"
            }
        ],
        "subnets": [
            {
                "id": "subnet-7f5da419",
                "zone": "us-west-1a",
                "vpcId": "vpc-f0979997"
            },
            {
                "id": "subnet-9c9287c7",
                "zone": "us-west-1c",
                "vpcId": "vpc-f0979997"
            }
        ],
        "instanceFamilies": [
            "optimal",
            "c1",
            "c3",
            "c4",
            "c5",
            "c5a",
            "c5d",
            "c5n",
            "c6g",
            "d2",
            "g2",
            "g3",
            "g4dn",
            "i2",
            "i3",
            "i3en",
            "m1",
            "m2",
            "m3",
            "m4",
            "m5",
            "m5a",
            "m5ad",
            "m5d",
            "m6g",
            "r3",
            "r4",
            "r5",
            "r5a",
            "r5ad",
            "r5d",
            "r6g",
            "z1d"
        ],
        "allocStrategy": [
            "BEST_FIT",
            "BEST_FIT_PROGRESSIVE",
            "SPOT_CAPACITY_OPTIMIZED"
        ]
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

