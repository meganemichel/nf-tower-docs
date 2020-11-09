---
title: Workflow
weight: 1
layout: single
publishdate: 2020-11-09 07:00:00 +0000
authors:
  - "Evan Floden"
  - "Julio Fernandez"
  - "Seqera Labs"
headline: 'Workflow'
description: 'API Workflow'
menu:
  docs:
    parent: API
    weight: 2

------------------------------------------------------------------------------------------------

## List workflows
List workflow records matching the filter parameters.

{{% tip %}}
**GET** /api/workflows
{{% /tip %}}

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflows'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
	"workflows": [{
		"workflow": {
			"id": "TWl5sx6tUPCiZ",
			"submit": "2020-11-12T09:22:41Z",
			"start": "2020-11-12T09:23:17Z",
			"complete": "2020-11-12T09:27:26.594Z",
			"dateCreated": "2020-11-12T09:22:41Z",
			"lastUpdated": "2020-11-12T09:27:27.052792Z",
			"runName": "naughty_booth",
			"sessionId": "3b029c2a-b8e1-463b-8e1a-755397d842fd",
			"profile": "standard",
			"workDir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ",
			"commitId": "708cf52a33a839b56aa48f39c42e27415d2d4c59",
			"userName": "user1",
			"scriptId": "29dd20fd3f4d3de2f8fbf0149713be5a",
			"revision": "master",
			"commandLine": "nextflow run 'https://github.com/nextflow-io/rnaseq-nf' -name naughty_booth -with-tower 'https://staging-tower.xyz/api'",
			"projectName": "nextflow-io/rnaseq-nf",
			"scriptName": "main.nf",
			"launchId": "XK997uw5I9MUX9u1Ct4k9",
			"status": "SUCCEEDED",
			"configFiles": [
				"/.nextflow/assets/nextflow-io/rnaseq-nf/nextflow.config",
				"/nextflow.config"
			],
			"params": {
				"reads": "/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/*_{1,2}.fq",
				"multiqc": "/.nextflow/assets/nextflow-io/rnaseq-nf/multiqc",
				"transcriptome": "/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa",
				"outdir": "results"
			},
			"configText": "manifest {\n   description = 'Proof of concept of a RNA-seq pipeline implemented with Nextflow'\n   author = 'Paolo Di Tommaso'\n   nextflowVersion = '>=20.07.0'\n}\n\nprocess {\n   container = 'quay.io/nextflow/rnaseq-nf:latest'\n   executor = 'awsbatch'\n   queue = 'TowerForge-7R2IwqiZyCmMDhgHxlymNY-work'\n}\n\naws {\n   region = 'eu-west-1'\n   client {\n      uploadChunkSize = 10485760\n   }\n   batch {\n      cliPath = '/home/ec2-user/miniconda/bin/aws'\n   }\n}\n\nworkDir = 's3://random-tower-bucket/scratch/TWl5sx6tUPCiZ'\nrunName = 'naughty_booth'\n\ntower {\n   enabled = true\n   endpoint = 'https://staging-tower.xyz/api'\n}\n",
			"manifest": {
				"nextflowVersion": ">=20.07.0",
				"defaultBranch": "master",
				"version": null,
				"homePage": null,
				"gitmodules": null,
				"description": "Proof of concept of a RNA-seq pipeline implemented with Nextflow",
				"name": null,
				"mainScript": "main.nf",
				"author": "Paolo Di Tommaso"
			},
			"nextflow": {
				"version": "20.10.0",
				"build": "5430",
				"timestamp": "2020-11-01T15:14:00Z"
			},
			"stats": {
				"computeTimeFmt": "(a few seconds)",
				"cachedCount": 0,
				"failedCount": 0,
				"ignoredCount": 0,
				"succeedCount": 6,
				"cachedCountFmt": "0",
				"succeedCountFmt": "6",
				"failedCountFmt": "0",
				"ignoredCountFmt": "0",
				"cachedPct": 0.0,
				"failedPct": 0.0,
				"succeedPct": 100.0,
				"ignoredPct": 0.0,
				"cachedDuration": 0,
				"failedDuration": 0,
				"succeedDuration": 36924
			},
			"errorMessage": null,
			"errorReport": null,
			"deleted": null,
			"peakLoadCpus": null,
			"peakLoadTasks": null,
			"peakLoadMemory": null,
			"projectDir": "/.nextflow/assets/nextflow-io/rnaseq-nf",
			"homeDir": "/root",
			"container": "quay.io/nextflow/rnaseq-nf:latest",
			"repository": "https://github.com/nextflow-io/rnaseq-nf",
			"containerEngine": null,
			"scriptFile": "/.nextflow/assets/nextflow-io/rnaseq-nf/main.nf",
			"launchDir": "/",
			"duration": 253142,
			"exitStatus": 0,
			"resume": false,
			"success": true,
			"ownerId": 203
		},
		"progress": null
	}]
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

------------------------------------------------------------------------------------------------

## Describe workflow
Describe the workflow entity for a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
	"workflow": {
		"id": "TWl5sx6tUPCiZ",
		"submit": "2020-11-12T09:22:41Z",
		"start": "2020-11-12T09:23:17Z",
		"complete": "2020-11-12T09:27:27Z",
		"dateCreated": "2020-11-12T09:22:41Z",
		"lastUpdated": "2020-11-12T09:27:27Z",
		"runName": "naughty_booth",
		"sessionId": "3b029c2a-b8e1-463b-8e1a-755397d842fd",
		"profile": "standard",
		"workDir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ",
		"commitId": "708cf52a33a839b56aa48f39c42e27415d2d4c59",
		"userName": "user1",
		"scriptId": "29dd20fd3f4d3de2f8fbf0149713be5a",
		"revision": "master",
		"commandLine": "nextflow run 'https://github.com/nextflow-io/rnaseq-nf' -name naughty_booth -with-tower 'https://staging-tower.xyz/api'",
		"projectName": "nextflow-io/rnaseq-nf",
		"scriptName": "main.nf",
		"launchId": "XK997uw5I9MUX9u1Ct4k9",
		"status": "SUCCEEDED",
		"configFiles": [
			"/.nextflow/assets/nextflow-io/rnaseq-nf/nextflow.config",
			"/nextflow.config"
		],
		"params": {
			"transcriptome": "/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa",
			"reads": "/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/*_{1,2}.fq",
			"multiqc": "/.nextflow/assets/nextflow-io/rnaseq-nf/multiqc",
			"outdir": "results"
		},
		"configText": "manifest {\n   description = 'Proof of concept of a RNA-seq pipeline implemented with Nextflow'\n   author = 'Paolo Di Tommaso'\n   nextflowVersion = '>=20.07.0'\n}\n\nprocess {\n   container = 'quay.io/nextflow/rnaseq-nf:latest'\n   executor = 'awsbatch'\n   queue = 'TowerForge-7R2IwqiZyCmMDhgHxlymNY-work'\n}\n\naws {\n   region = 'eu-west-1'\n   client {\n      uploadChunkSize = 10485760\n   }\n   batch {\n      cliPath = '/home/ec2-user/miniconda/bin/aws'\n   }\n}\n\nworkDir = 's3://random-tower-bucket/scratch/TWl5sx6tUPCiZ'\nrunName = 'naughty_booth'\n\ntower {\n   enabled = true\n   endpoint = 'https://staging-tower.xyz/api'\n}\n",
		"manifest": {
			"nextflowVersion": ">=20.07.0",
			"defaultBranch": "master",
			"version": null,
			"homePage": null,
			"gitmodules": null,
			"description": "Proof of concept of a RNA-seq pipeline implemented with Nextflow",
			"name": null,
			"mainScript": "main.nf",
			"author": "Paolo Di Tommaso"
		},
		"nextflow": {
			"version": "20.10.0",
			"build": "5430",
			"timestamp": "2020-11-01T15:14:00Z"
		},
		"stats": {
			"computeTimeFmt": "(a few seconds)",
			"cachedCount": 0,
			"failedCount": 0,
			"ignoredCount": 0,
			"succeedCount": 6,
			"cachedCountFmt": "0",
			"succeedCountFmt": "6",
			"failedCountFmt": "0",
			"ignoredCountFmt": "0",
			"cachedPct": 0.0,
			"failedPct": 0.0,
			"succeedPct": 100.0,
			"ignoredPct": 0.0,
			"cachedDuration": 0,
			"failedDuration": 0,
			"succeedDuration": 36924
		},
		"errorMessage": null,
		"errorReport": null,
		"deleted": null,
		"peakLoadCpus": null,
		"peakLoadTasks": null,
		"peakLoadMemory": null,
		"projectDir": "/.nextflow/assets/nextflow-io/rnaseq-nf",
		"homeDir": "/root",
		"container": "quay.io/nextflow/rnaseq-nf:latest",
		"repository": "https://github.com/nextflow-io/rnaseq-nf",
		"containerEngine": null,
		"scriptFile": "/.nextflow/assets/nextflow-io/rnaseq-nf/main.nf",
		"launchDir": "/",
		"duration": 253142,
		"exitStatus": 0,
		"resume": false,
		"success": true,
		"ownerId": 203
	},
	"progress": {
		"workflowProgress": {
			"cpus": 6,
			"cpuTime": 36924,
			"cpuLoad": 22345,
			"memoryRss": 511905792,
			"memoryReq": 0,
			"readBytes": 106584422,
			"writeBytes": 8797176,
			"volCtxSwitch": 1802,
			"invCtxSwitch": 1015,
			"cost": 0.0002044445,
			"loadTasks": 0,
			"loadCpus": 0,
			"loadMemory": 0,
			"peakCpus": 2,
			"peakTasks": 2,
			"peakMemory": 0,
			"executors": [
				"awsbatch"
			],
			"dateCreated": "2020-11-12T09:30:52Z",
			"lastUpdated": "2020-11-12T09:30:52Z",
			"cached": 0,
			"pending": 0,
			"submitted": 0,
			"running": 0,
			"succeeded": 6,
			"failed": 0,
			"memoryEfficiency": 0.0,
			"cpuEfficiency": 60.516197
		},
		"processesProgress": [{
				"process": "RNASEQ:INDEX",
				"cpus": 0,
				"cpuTime": 0,
				"cpuLoad": 0,
				"memoryRss": 0,
				"memoryReq": 0,
				"readBytes": 0,
				"writeBytes": 0,
				"volCtxSwitch": 0,
				"invCtxSwitch": 0,
				"loadTasks": 0,
				"loadCpus": 0,
				"loadMemory": 0,
				"peakCpus": 1,
				"peakTasks": 1,
				"peakMemory": 0,
				"dateCreated": "2020-11-12T09:30:52Z",
				"lastUpdated": "2020-11-12T09:30:52Z",
				"cached": 0,
				"pending": 0,
				"submitted": 0,
				"running": 0,
				"succeeded": 1,
				"failed": 0,
				"memoryEfficiency": 0.0,
				"cpuEfficiency": 0.0
			},
			{
				"process": "RNASEQ:FASTQC",
				"cpus": 0,
				"cpuTime": 0,
				"cpuLoad": 0,
				"memoryRss": 0,
				"memoryReq": 0,
				"readBytes": 0,
				"writeBytes": 0,
				"volCtxSwitch": 0,
				"invCtxSwitch": 0,
				"loadTasks": 0,
				"loadCpus": 0,
				"loadMemory": 0,
				"peakCpus": 2,
				"peakTasks": 2,
				"peakMemory": 0,
				"dateCreated": "2020-11-12T09:30:52Z",
				"lastUpdated": "2020-11-12T09:30:52Z",
				"cached": 0,
				"pending": 0,
				"submitted": 0,
				"running": 0,
				"succeeded": 2,
				"failed": 0,
				"memoryEfficiency": 0.0,
				"cpuEfficiency": 0.0
			},
			{
				"process": "RNASEQ:QUANT",
				"cpus": 0,
				"cpuTime": 0,
				"cpuLoad": 0,
				"memoryRss": 0,
				"memoryReq": 0,
				"readBytes": 0,
				"writeBytes": 0,
				"volCtxSwitch": 0,
				"invCtxSwitch": 0,
				"loadTasks": 0,
				"loadCpus": 0,
				"loadMemory": 0,
				"peakCpus": 2,
				"peakTasks": 2,
				"peakMemory": 0,
				"dateCreated": "2020-11-12T09:30:52Z",
				"lastUpdated": "2020-11-12T09:30:52Z",
				"cached": 0,
				"pending": 0,
				"submitted": 0,
				"running": 0,
				"succeeded": 2,
				"failed": 0,
				"memoryEfficiency": 0.0,
				"cpuEfficiency": 0.0
			},
			{
				"process": "MULTIQC",
				"cpus": 0,
				"cpuTime": 0,
				"cpuLoad": 0,
				"memoryRss": 0,
				"memoryReq": 0,
				"readBytes": 0,
				"writeBytes": 0,
				"volCtxSwitch": 0,
				"invCtxSwitch": 0,
				"loadTasks": 0,
				"loadCpus": 0,
				"loadMemory": 0,
				"peakCpus": 1,
				"peakTasks": 1,
				"peakMemory": 0,
				"dateCreated": "2020-11-12T09:30:52Z",
				"lastUpdated": "2020-11-12T09:30:52Z",
				"cached": 0,
				"pending": 0,
				"submitted": 0,
				"running": 0,
				"succeeded": 1,
				"failed": 0,
				"memoryEfficiency": 0.0,
				"cpuEfficiency": 0.0
			}
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

------------------------------------------------------------------------------------------------

## Describe workflow progress
Retrieve the execution progress for a given workflow ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/progress
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/progress'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "progress": {
        "workflowProgress": {
            "cpus": 6,
            "cpuTime": 36924,
            "cpuLoad": 22345,
            "memoryRss": 511905792,
            "memoryReq": 0,
            "readBytes": 106584422,
            "writeBytes": 8797176,
            "volCtxSwitch": 1802,
            "invCtxSwitch": 1015,
            "cost": 0.0002044445,
            "loadTasks": 0,
            "loadCpus": 0,
            "loadMemory": 0,
            "peakCpus": 2,
            "peakTasks": 2,
            "peakMemory": 0,
            "executors": [
                "awsbatch"
            ],
            "dateCreated": "2020-11-12T09:30:52Z",
            "lastUpdated": "2020-11-12T09:30:52Z",
            "cached": 0,
            "pending": 0,
            "submitted": 0,
            "running": 0,
            "succeeded": 6,
            "failed": 0,
            "memoryEfficiency": 0.0,
            "cpuEfficiency": 60.516197
        },
        "processesProgress": [
            {
                "process": "RNASEQ:INDEX",
                "cpus": 0,
                "cpuTime": 0,
                "cpuLoad": 0,
                "memoryRss": 0,
                "memoryReq": 0,
                "readBytes": 0,
                "writeBytes": 0,
                "volCtxSwitch": 0,
                "invCtxSwitch": 0,
                "loadTasks": 0,
                "loadCpus": 0,
                "loadMemory": 0,
                "peakCpus": 1,
                "peakTasks": 1,
                "peakMemory": 0,
                "dateCreated": "2020-11-12T09:30:52Z",
                "lastUpdated": "2020-11-12T09:30:52Z",
                "cached": 0,
                "pending": 0,
                "submitted": 0,
                "running": 0,
                "succeeded": 1,
                "failed": 0,
                "memoryEfficiency": 0.0,
                "cpuEfficiency": 0.0
            },
            {
                "process": "RNASEQ:FASTQC",
                "cpus": 0,
                "cpuTime": 0,
                "cpuLoad": 0,
                "memoryRss": 0,
                "memoryReq": 0,
                "readBytes": 0,
                "writeBytes": 0,
                "volCtxSwitch": 0,
                "invCtxSwitch": 0,
                "loadTasks": 0,
                "loadCpus": 0,
                "loadMemory": 0,
                "peakCpus": 2,
                "peakTasks": 2,
                "peakMemory": 0,
                "dateCreated": "2020-11-12T09:30:52Z",
                "lastUpdated": "2020-11-12T09:30:52Z",
                "cached": 0,
                "pending": 0,
                "submitted": 0,
                "running": 0,
                "succeeded": 2,
                "failed": 0,
                "memoryEfficiency": 0.0,
                "cpuEfficiency": 0.0
            },
            {
                "process": "RNASEQ:QUANT",
                "cpus": 0,
                "cpuTime": 0,
                "cpuLoad": 0,
                "memoryRss": 0,
                "memoryReq": 0,
                "readBytes": 0,
                "writeBytes": 0,
                "volCtxSwitch": 0,
                "invCtxSwitch": 0,
                "loadTasks": 0,
                "loadCpus": 0,
                "loadMemory": 0,
                "peakCpus": 2,
                "peakTasks": 2,
                "peakMemory": 0,
                "dateCreated": "2020-11-12T09:30:52Z",
                "lastUpdated": "2020-11-12T09:30:52Z",
                "cached": 0,
                "pending": 0,
                "submitted": 0,
                "running": 0,
                "succeeded": 2,
                "failed": 0,
                "memoryEfficiency": 0.0,
                "cpuEfficiency": 0.0
            },
            {
                "process": "MULTIQC",
                "cpus": 0,
                "cpuTime": 0,
                "cpuLoad": 0,
                "memoryRss": 0,
                "memoryReq": 0,
                "readBytes": 0,
                "writeBytes": 0,
                "volCtxSwitch": 0,
                "invCtxSwitch": 0,
                "loadTasks": 0,
                "loadCpus": 0,
                "loadMemory": 0,
                "peakCpus": 1,
                "peakTasks": 1,
                "peakMemory": 0,
                "dateCreated": "2020-11-12T09:30:52Z",
                "lastUpdated": "2020-11-12T09:30:52Z",
                "cached": 0,
                "pending": 0,
                "submitted": 0,
                "running": 0,
                "succeeded": 1,
                "failed": 0,
                "memoryEfficiency": 0.0,
                "cpuEfficiency": 0.0
            }
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

------------------------------------------------------------------------------------------------

## List workflow tasks
List the tasks for a given workflow ID and filter parameters.

{{% tip %}}
**GET** /api/workflow/:workflowId/tasks
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/tasks'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "tasks": [
        {
            "task": {
                "id": 937358,
                "taskId": 1,
                "status": "COMPLETED",
                "dateCreated": "2020-11-12T09:23:20Z",
                "lastUpdated": "2020-11-12T09:26:17Z",
                "disk": null,
                "start": "2020-11-12T09:26:06Z",
                "submit": "2020-11-12T09:23:20Z",
                "container": "quay.io/nextflow/rnaseq-nf:latest",
                "complete": "2020-11-12T09:26:16Z",
                "exitStatus": 0,
                "hash": "07/d3280f",
                "attempt": 1,
                "scratch": null,
                "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/07/d3280fde2a451349aef6b7f9d1ea49",
                "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "cpus": 1,
                "memory": null,
                "executor": "awsbatch",
                "machineType": "r4.large",
                "cloudZone": "eu-west-1b",
                "priceModel": "spot",
                "cost": 0.0000051111,
                "errorAction": null,
                "realtime": 641,
                "nativeId": "505d7cb4-3a79-4782-bed9-17953a79956b",
                "pcpu": 53.0,
                "pmem": 0.0,
                "rss": 10686464,
                "vmem": 43692032,
                "peakRss": 10686464,
                "peakVmem": 43724800,
                "rchar": 2563280,
                "wchar": 1908275,
                "syscr": 323,
                "syscw": 1188,
                "readBytes": 17907712,
                "writeBytes": 1949696,
                "volCtxt": 17,
                "invCtxt": 4,
                "process": "RNASEQ:INDEX",
                "script": "\nsalmon index --threads 1 -t ggal_1_48850000_49020000.Ggal71.500bpflank.fa -i index\n",
                "time": null,
                "duration": 176609,
                "module": [],
                "name": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "tag": "ggal_1_48850000_49020000",
                "env": null,
                "exit": "0"
            }
        },
        {
            "task": {
                "id": 937356,
                "taskId": 2,
                "status": "COMPLETED",
                "dateCreated": "2020-11-12T09:23:19Z",
                "lastUpdated": "2020-11-12T09:25:57Z",
                "disk": null,
                "start": "2020-11-12T09:25:26Z",
                "submit": "2020-11-12T09:23:20Z",
                "container": "quay.io/nextflow/rnaseq-nf:latest",
                "complete": "2020-11-12T09:25:56Z",
                "exitStatus": 0,
                "hash": "82/a45d22",
                "attempt": 1,
                "scratch": null,
                "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/82/a45d22cf066db3af29be15533550c5",
                "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "cpus": 1,
                "memory": null,
                "executor": "awsbatch",
                "machineType": "r4.large",
                "cloudZone": "eu-west-1b",
                "priceModel": "spot",
                "cost": 0.0000715556,
                "errorAction": null,
                "realtime": 13109,
                "nativeId": "9850d10d-2269-4ed4-ad9c-f7c0f5572d63",
                "pcpu": 61.4,
                "pmem": 0.8,
                "rss": 135368704,
                "vmem": 2567163904,
                "peakRss": 135368704,
                "peakVmem": 2619355136,
                "rchar": 36526325,
                "wchar": 2058227,
                "syscr": 9132,
                "syscw": 9176,
                "readBytes": 81121280,
                "writeBytes": 39149568,
                "volCtxt": 37,
                "invCtxt": 22,
                "process": "RNASEQ:FASTQC",
                "script": "\nmkdir fastqc_ggal_liver_logs\nfastqc -o fastqc_ggal_liver_logs -f fastq -q ggal_liver_1.fq ggal_liver_2.fq\n",
                "time": null,
                "duration": 156673,
                "module": [],
                "name": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "tag": "FASTQC on ggal_liver",
                "env": null,
                "exit": "0"
            }
        },
        {
            "task": {
                "id": 937357,
                "taskId": 3,
                "status": "COMPLETED",
                "dateCreated": "2020-11-12T09:23:20Z",
                "lastUpdated": "2020-11-12T09:26:08Z",
                "disk": null,
                "start": "2020-11-12T09:25:27Z",
                "submit": "2020-11-12T09:23:20Z",
                "container": "quay.io/nextflow/rnaseq-nf:latest",
                "complete": "2020-11-12T09:26:06Z",
                "exitStatus": 0,
                "hash": "4a/3f5108",
                "attempt": 1,
                "scratch": null,
                "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/4a/3f5108bbfe6eb9b224bd1cc1c1ae49",
                "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "cpus": 1,
                "memory": null,
                "executor": "awsbatch",
                "machineType": "r4.large",
                "cloudZone": "eu-west-1b",
                "priceModel": "spot",
                "cost": 0.0000868889,
                "errorAction": null,
                "realtime": 16164,
                "nativeId": "0d8d9f28-942e-4644-bb94-ce0a7596f70e",
                "pcpu": 60.6,
                "pmem": 1.1,
                "rss": 183484416,
                "vmem": 2567163904,
                "peakRss": 183484416,
                "peakVmem": 2619355136,
                "rchar": 36211687,
                "wchar": 2058047,
                "syscr": 9060,
                "syscw": 9176,
                "readBytes": 81121280,
                "writeBytes": 39153664,
                "volCtxt": 39,
                "invCtxt": 27,
                "process": "RNASEQ:FASTQC",
                "script": "\nmkdir fastqc_ggal_gut_logs\nfastqc -o fastqc_ggal_gut_logs -f fastq -q ggal_gut_1.fq ggal_gut_2.fq\n",
                "time": null,
                "duration": 166666,
                "module": [],
                "name": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "tag": "FASTQC on ggal_gut",
                "env": null,
                "exit": "0"
            }
        },
        {
            "task": {
                "id": 937359,
                "taskId": 4,
                "status": "COMPLETED",
                "dateCreated": "2020-11-12T09:26:18Z",
                "lastUpdated": "2020-11-12T09:26:57Z",
                "disk": null,
                "start": "2020-11-12T09:26:36Z",
                "submit": "2020-11-12T09:26:17Z",
                "container": "quay.io/nextflow/rnaseq-nf:latest",
                "complete": "2020-11-12T09:26:56Z",
                "exitStatus": 0,
                "hash": "21/c84367",
                "attempt": 1,
                "scratch": null,
                "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/21/c8436703a65c74bc1fd57f2e07edd3",
                "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "cpus": 1,
                "memory": null,
                "executor": "awsbatch",
                "machineType": "r4.large",
                "cloudZone": "eu-west-1b",
                "priceModel": "spot",
                "cost": 0.0000051111,
                "errorAction": null,
                "realtime": 946,
                "nativeId": "af9934a0-4c69-4676-8cc8-32a8d80a9a37",
                "pcpu": 33.2,
                "pmem": 0.0,
                "rss": 10915840,
                "vmem": 43692032,
                "peakRss": 10915840,
                "peakVmem": 43724800,
                "rchar": 2069919,
                "wchar": 22531,
                "syscr": 286,
                "syscw": 101,
                "readBytes": 17907712,
                "writeBytes": 90112,
                "volCtxt": 18,
                "invCtxt": 4,
                "process": "RNASEQ:QUANT",
                "script": "\nsalmon quant --threads 1 --libType=U -i index -1 ggal_liver_1.fq -2 ggal_liver_2.fq -o ggal_liver\n",
                "time": null,
                "duration": 39341,
                "module": [],
                "name": "RNASEQ:QUANT (ggal_liver)",
                "tag": "ggal_liver",
                "env": null,
                "exit": "0"
            }
        },
        {
            "task": {
                "id": 937360,
                "taskId": 5,
                "status": "COMPLETED",
                "dateCreated": "2020-11-12T09:26:18Z",
                "lastUpdated": "2020-11-12T09:26:58Z",
                "disk": null,
                "start": "2020-11-12T09:26:36Z",
                "submit": "2020-11-12T09:26:17Z",
                "container": "quay.io/nextflow/rnaseq-nf:latest",
                "complete": "2020-11-12T09:26:57Z",
                "exitStatus": 0,
                "hash": "9a/07ec7b",
                "attempt": 1,
                "scratch": null,
                "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/9a/07ec7b41243c1f48ef832292959b14",
                "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "cpus": 1,
                "memory": null,
                "executor": "awsbatch",
                "machineType": "r4.large",
                "cloudZone": "eu-west-1b",
                "priceModel": "spot",
                "cost": 0.0000051111,
                "errorAction": null,
                "realtime": 981,
                "nativeId": "b716a7a5-ce75-4bc1-92f6-d41776537e13",
                "pcpu": 31.9,
                "pmem": 0.3,
                "rss": 54317056,
                "vmem": 131776512,
                "peakRss": 54317056,
                "peakVmem": 131792896,
                "rchar": 2069913,
                "wchar": 22515,
                "syscr": 286,
                "syscw": 101,
                "readBytes": 17907712,
                "writeBytes": 73728,
                "volCtxt": 19,
                "invCtxt": 25,
                "process": "RNASEQ:QUANT",
                "script": "\nsalmon quant --threads 1 --libType=U -i index -1 ggal_gut_1.fq -2 ggal_gut_2.fq -o ggal_gut\n",
                "time": null,
                "duration": 39285,
                "module": [],
                "name": "RNASEQ:QUANT (ggal_gut)",
                "tag": "ggal_gut",
                "env": null,
                "exit": "0"
            }
        },
        {
            "task": {
                "id": 937361,
                "taskId": 6,
                "status": "COMPLETED",
                "dateCreated": "2020-11-12T09:26:58Z",
                "lastUpdated": "2020-11-12T09:27:27Z",
                "disk": null,
                "start": "2020-11-12T09:27:06Z",
                "submit": "2020-11-12T09:26:57Z",
                "container": "quay.io/nextflow/rnaseq-nf:latest",
                "complete": "2020-11-12T09:27:26Z",
                "exitStatus": 0,
                "hash": "2f/08de3e",
                "attempt": 1,
                "scratch": null,
                "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/2f/08de3ebab6bb65574c47d3eca6040d",
                "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "cpus": 1,
                "memory": null,
                "executor": "awsbatch",
                "machineType": "r4.large",
                "cloudZone": "eu-west-1b",
                "priceModel": "spot",
                "cost": 0.0000306667,
                "errorAction": null,
                "realtime": 5083,
                "nativeId": "a3b4f327-9ba3-439f-b268-9d8ff0ccc8e2",
                "pcpu": 69.6,
                "pmem": 0.7,
                "rss": 117133312,
                "vmem": 400863232,
                "peakRss": 117133312,
                "peakVmem": 457957376,
                "rchar": 27143298,
                "wchar": 2727581,
                "syscr": 9161,
                "syscw": 316,
                "readBytes": 147369984,
                "writeBytes": 3399680,
                "volCtxt": 1672,
                "invCtxt": 933,
                "process": "MULTIQC",
                "script": "\ncp multiqc/* .\necho \"custom_logo: $PWD/logo.png\" >> multiqc_config.yaml\nmultiqc .\n",
                "time": null,
                "duration": 29316,
                "module": [],
                "name": "MULTIQC",
                "tag": null,
                "env": null,
                "exit": "0"
            }
        }
    ],
    "total": 6
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

## Describe workflow task
Describe a task entity with a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/tasks/:taskId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |
| `taskId` | `int` | query | **Required**. Workflow task id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/task/{taskId}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "task": {
        "id": 937358,
        "taskId": 1,
        "status": "COMPLETED",
        "dateCreated": "2020-11-12T09:23:20Z",
        "lastUpdated": "2020-11-12T09:26:17Z",
        "disk": null,
        "start": "2020-11-12T09:26:06Z",
        "submit": "2020-11-12T09:23:20Z",
        "container": "quay.io/nextflow/rnaseq-nf:latest",
        "complete": "2020-11-12T09:26:16Z",
        "exitStatus": 0,
        "hash": "07/d3280f",
        "attempt": 1,
        "scratch": null,
        "workdir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ/07/d3280fde2a451349aef6b7f9d1ea49",
        "queue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
        "cpus": 1,
        "memory": null,
        "executor": "awsbatch",
        "machineType": "r4.large",
        "cloudZone": "eu-west-1b",
        "priceModel": "spot",
        "cost": 0.0000051111,
        "errorAction": null,
        "realtime": 641,
        "nativeId": "505d7cb4-3a79-4782-bed9-17953a79956b",
        "pcpu": 53.0,
        "pmem": 0.0,
        "rss": 10686464,
        "vmem": 43692032,
        "peakRss": 10686464,
        "peakVmem": 43724800,
        "rchar": 2563280,
        "wchar": 1908275,
        "syscr": 323,
        "syscw": 1188,
        "readBytes": 17907712,
        "writeBytes": 1949696,
        "volCtxt": 17,
        "invCtxt": 4,
        "process": "RNASEQ:INDEX",
        "script": "\nsalmon index --threads 1 -t ggal_1_48850000_49020000.Ggal71.500bpflank.fa -i index\n",
        "time": null,
        "duration": 176609,
        "module": [],
        "name": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
        "tag": "ggal_1_48850000_49020000",
        "env": null,
        "exit": "0"
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

## Describe workflow execution metrics
Get the execution metrics for a given workflow ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/metrics
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/metrics'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "metrics": [
        {
            "id": 5326,
            "process": "FASTQC",
            "cpu": {
                "mean": 61.0,
                "min": 60.6,
                "q1": 60.8,
                "q2": 61.0,
                "q3": 61.2,
                "max": 61.4,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "warnings": []
            },
            "mem": {
                "mean": 1.59427008E8,
                "min": 1.35368992E8,
                "q1": 1.47398E8,
                "q2": 1.59427008E8,
                "q3": 1.71455008E8,
                "max": 1.83484E8,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "warnings": []
            },
            "vmem": {
                "mean": 2.61936E9,
                "min": 2.61936E9,
                "q1": 2.61936E9,
                "q2": 2.61936E9,
                "q3": 2.61936E9,
                "max": 2.61936E9,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "warnings": []
            },
            "time": {
                "mean": 14636.5,
                "min": 13109.0,
                "q1": 13872.8,
                "q2": 14636.5,
                "q3": 15400.2,
                "max": 16164.0,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "warnings": []
            },
            "reads": {
                "mean": 3.6369E7,
                "min": 3.62117E7,
                "q1": 3.62903E7,
                "q2": 3.6369E7,
                "q3": 3.64477E7,
                "max": 3.65263E7,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "warnings": []
            },
            "writes": {
                "mean": 2058140.0,
                "min": 2058050.0,
                "q1": 2058090.0,
                "q2": 2058140.0,
                "q3": 2058180.0,
                "max": 2058230.0,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "warnings": []
            },
            "cpuUsage": {
                "mean": 61.0,
                "min": 60.6,
                "q1": 60.8,
                "q2": 61.0,
                "q3": 61.2,
                "max": 61.4,
                "minLabel": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "maxLabel": "RNASEQ:FASTQC (FASTQC on ggal_liver)",
                "q1Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q2Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "q3Label": "RNASEQ:FASTQC (FASTQC on ggal_gut)",
                "warnings": []
            },
            "memUsage": null,
            "timeUsage": null
        },
        {
            "id": 5327,
            "process": "INDEX",
            "cpu": {
                "mean": 53.0,
                "min": 53.0,
                "q1": 53.0,
                "q2": 53.0,
                "q3": 53.0,
                "max": 53.0,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "mem": {
                "mean": 1.06865E7,
                "min": 1.06865E7,
                "q1": 1.06865E7,
                "q2": 1.06865E7,
                "q3": 1.06865E7,
                "max": 1.06865E7,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "vmem": {
                "mean": 4.37248E7,
                "min": 4.37248E7,
                "q1": 4.37248E7,
                "q2": 4.37248E7,
                "q3": 4.37248E7,
                "max": 4.37248E7,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "time": {
                "mean": 641.0,
                "min": 641.0,
                "q1": 641.0,
                "q2": 641.0,
                "q3": 641.0,
                "max": 641.0,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "reads": {
                "mean": 2563280.0,
                "min": 2563280.0,
                "q1": 2563280.0,
                "q2": 2563280.0,
                "q3": 2563280.0,
                "max": 2563280.0,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "writes": {
                "mean": 1908280.0,
                "min": 1908280.0,
                "q1": 1908280.0,
                "q2": 1908280.0,
                "q3": 1908280.0,
                "max": 1908280.0,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "cpuUsage": {
                "mean": 53.0,
                "min": 53.0,
                "q1": 53.0,
                "q2": 53.0,
                "q3": 53.0,
                "max": 53.0,
                "minLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "maxLabel": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q1Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q2Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "q3Label": "RNASEQ:INDEX (ggal_1_48850000_49020000)",
                "warnings": []
            },
            "memUsage": null,
            "timeUsage": null
        },
        {
            "id": 5328,
            "process": "QUANT",
            "cpu": {
                "mean": 32.55,
                "min": 31.9,
                "q1": 32.23,
                "q2": 32.55,
                "q3": 32.88,
                "max": 33.2,
                "minLabel": "RNASEQ:QUANT (ggal_gut)",
                "maxLabel": "RNASEQ:QUANT (ggal_liver)",
                "q1Label": "RNASEQ:QUANT (ggal_gut)",
                "q2Label": "RNASEQ:QUANT (ggal_gut)",
                "q3Label": "RNASEQ:QUANT (ggal_gut)",
                "warnings": []
            },
            "mem": {
                "mean": 3.26164E7,
                "min": 1.09158E7,
                "q1": 2.17661E7,
                "q2": 3.26164E7,
                "q3": 4.34668E7,
                "max": 5.43171E7,
                "minLabel": "RNASEQ:QUANT (ggal_liver)",
                "maxLabel": "RNASEQ:QUANT (ggal_gut)",
                "q1Label": "RNASEQ:QUANT (ggal_liver)",
                "q2Label": "RNASEQ:QUANT (ggal_liver)",
                "q3Label": "RNASEQ:QUANT (ggal_liver)",
                "warnings": []
            },
            "vmem": {
                "mean": 8.77588E7,
                "min": 4.37248E7,
                "q1": 6.57418E7,
                "q2": 8.77588E7,
                "q3": 1.09776E8,
                "max": 1.31793E8,
                "minLabel": "RNASEQ:QUANT (ggal_liver)",
                "maxLabel": "RNASEQ:QUANT (ggal_gut)",
                "q1Label": "RNASEQ:QUANT (ggal_liver)",
                "q2Label": "RNASEQ:QUANT (ggal_liver)",
                "q3Label": "RNASEQ:QUANT (ggal_liver)",
                "warnings": []
            },
            "time": {
                "mean": 963.5,
                "min": 946.0,
                "q1": 954.75,
                "q2": 963.5,
                "q3": 972.25,
                "max": 981.0,
                "minLabel": "RNASEQ:QUANT (ggal_liver)",
                "maxLabel": "RNASEQ:QUANT (ggal_gut)",
                "q1Label": "RNASEQ:QUANT (ggal_liver)",
                "q2Label": "RNASEQ:QUANT (ggal_liver)",
                "q3Label": "RNASEQ:QUANT (ggal_liver)",
                "warnings": []
            },
            "reads": {
                "mean": 2069920.0,
                "min": 2069910.0,
                "q1": 2069910.0,
                "q2": 2069920.0,
                "q3": 2069920.0,
                "max": 2069920.0,
                "minLabel": "RNASEQ:QUANT (ggal_gut)",
                "maxLabel": "RNASEQ:QUANT (ggal_liver)",
                "q1Label": "RNASEQ:QUANT (ggal_gut)",
                "q2Label": "RNASEQ:QUANT (ggal_gut)",
                "q3Label": "RNASEQ:QUANT (ggal_gut)",
                "warnings": []
            },
            "writes": {
                "mean": 22523.0,
                "min": 22515.0,
                "q1": 22519.0,
                "q2": 22523.0,
                "q3": 22527.0,
                "max": 22531.0,
                "minLabel": "RNASEQ:QUANT (ggal_gut)",
                "maxLabel": "RNASEQ:QUANT (ggal_liver)",
                "q1Label": "RNASEQ:QUANT (ggal_gut)",
                "q2Label": "RNASEQ:QUANT (ggal_gut)",
                "q3Label": "RNASEQ:QUANT (ggal_gut)",
                "warnings": []
            },
            "cpuUsage": {
                "mean": 32.55,
                "min": 31.9,
                "q1": 32.23,
                "q2": 32.55,
                "q3": 32.88,
                "max": 33.2,
                "minLabel": "RNASEQ:QUANT (ggal_gut)",
                "maxLabel": "RNASEQ:QUANT (ggal_liver)",
                "q1Label": "RNASEQ:QUANT (ggal_gut)",
                "q2Label": "RNASEQ:QUANT (ggal_gut)",
                "q3Label": "RNASEQ:QUANT (ggal_gut)",
                "warnings": []
            },
            "memUsage": null,
            "timeUsage": null
        },
        {
            "id": 5329,
            "process": "MULTIQC",
            "cpu": {
                "mean": 69.6,
                "min": 69.6,
                "q1": 69.6,
                "q2": 69.6,
                "q3": 69.6,
                "max": 69.6,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "mem": {
                "mean": 1.17133E8,
                "min": 1.17133E8,
                "q1": 1.17133E8,
                "q2": 1.17133E8,
                "q3": 1.17133E8,
                "max": 1.17133E8,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "vmem": {
                "mean": 4.57956992E8,
                "min": 4.57956992E8,
                "q1": 4.57956992E8,
                "q2": 4.57956992E8,
                "q3": 4.57956992E8,
                "max": 4.57956992E8,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "time": {
                "mean": 5083.0,
                "min": 5083.0,
                "q1": 5083.0,
                "q2": 5083.0,
                "q3": 5083.0,
                "max": 5083.0,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "reads": {
                "mean": 2.71433E7,
                "min": 2.71433E7,
                "q1": 2.71433E7,
                "q2": 2.71433E7,
                "q3": 2.71433E7,
                "max": 2.71433E7,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "writes": {
                "mean": 2727580.0,
                "min": 2727580.0,
                "q1": 2727580.0,
                "q2": 2727580.0,
                "q3": 2727580.0,
                "max": 2727580.0,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "cpuUsage": {
                "mean": 69.6,
                "min": 69.6,
                "q1": 69.6,
                "q2": 69.6,
                "q3": 69.6,
                "max": 69.6,
                "minLabel": "MULTIQC",
                "maxLabel": "MULTIQC",
                "q1Label": "MULTIQC",
                "q2Label": "MULTIQC",
                "q3Label": "MULTIQC",
                "warnings": []
            },
            "memUsage": null,
            "timeUsage": null
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
    "message": "Oops... Unable to process request - Error ID: 54apnFENQxbvCr23JaIjLb"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

------------------------------------------------------------------------------------------------

## Get workflow launch information
Relaunch a workflow entity with a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/launch
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/launch'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "launch": {
        "id": "XK997uw5I9MUX9u1Ct4k9",
        "computeEnv": {
            "id": "7R2IwqiZyCmMDhgHxlymNY",
            "name": "AWS Batch Spot (eu-westr-1)",
            "description": null,
            "platform": "aws-batch",
            "config": {
                "region": "eu-west-1",
                "computeQueue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-work",
                "computeJobRole": null,
                "headQueue": "TowerForge-7R2IwqiZyCmMDhgHxlymNY-head",
                "headJobRole": null,
                "cliPath": "/home/ec2-user/miniconda/bin/aws",
                "volumes": [],
                "workDir": "s3://random-tower-bucket",
                "preRunScript": null,
                "postRunScript": null,
                "forge": {
                    "type": "SPOT",
                    "minCpus": 1,
                    "maxCpus": 4,
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
                    "allowBuckets": [],
                    "ebsBlockSize": null
                },
                "forgedResources": [
                    {
                        "IamRole": "arn:aws:iam::718519895578:role/TowerForge-7R2IwqiZyCmMDhgHxlymNY-ServiceRole"
                    },
                    {
                        "IamRole": "arn:aws:iam::718519895578:role/TowerForge-7R2IwqiZyCmMDhgHxlymNY-FleetRole"
                    },
                    {
                        "IamInstanceProfile": "arn:aws:iam::718519895578:instance-profile/TowerForge-7R2IwqiZyCmMDhgHxlymNY-InstanceRole"
                    },
                    {
                        "Ec2LaunchTemplate": "TowerForge-7R2IwqiZyCmMDhgHxlymNY"
                    },
                    {
                        "BatchEnv": "arn:aws:batch:eu-west-1:718519895578:compute-environment/TowerForge-7R2IwqiZyCmMDhgHxlymNY-head"
                    },
                    {
                        "BatchQueue": "arn:aws:batch:eu-west-1:718519895578:job-queue/TowerForge-7R2IwqiZyCmMDhgHxlymNY-head"
                    },
                    {
                        "BatchEnv": "arn:aws:batch:eu-west-1:718519895578:compute-environment/TowerForge-7R2IwqiZyCmMDhgHxlymNY-work"
                    },
                    {
                        "BatchQueue": "arn:aws:batch:eu-west-1:718519895578:job-queue/TowerForge-7R2IwqiZyCmMDhgHxlymNY-work"
                    }
                ]
            },
            "dateCreated": "2020-11-12T08:45:03Z",
            "lastUpdated": "2020-11-12T08:46:09Z",
            "lastUsed": "2020-11-12T09:22:41Z",
            "deleted": null,
            "status": "AVAILABLE",
            "message": null,
            "primary": null,
            "credentialsId": "71dPE24OFUkp6BcADFFyZ3"
        },
        "pipeline": "https://github.com/nextflow-io/rnaseq-nf",
        "workDir": "s3://random-tower-bucket",
        "revision": null,
        "sessionId": "3b029c2a-b8e1-463b-8e1a-755397d842fd",
        "configProfiles": [],
        "configText": null,
        "paramsText": null,
        "preRunScript": null,
        "postRunScript": null,
        "mainScript": null,
        "entryName": null,
        "resume": false,
        "pullLatest": false,
        "resumeDir": "s3://random-tower-bucket/scratch/TWl5sx6tUPCiZ",
        "resumeCommitId": "708cf52a33a839b56aa48f39c42e27415d2d4c59",
        "dateCreated": "2020-11-12T09:22:39Z"
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

## Describe workflow log
Retrieves a workflow entity' log with a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/log
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/log'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "log": {
        "entries": [
            "N E X T F L O W  ~  version 20.10.0",
            "Pulling nextflow-io/rnaseq-nf ...",
            " downloaded from https://github.com/nextflow-io/rnaseq-nf.git",
            "Launching `nextflow-io/rnaseq-nf` [naughty_booth] - revision: 708cf52a33 [master]",
            " R N A S E Q - N F   P I P E L I N E",
            " ===================================",
            " transcriptome: /.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa",
            " reads        : /.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/*_{1,2}.fq",
            " outdir       : results",
            " ",
            "Monitor the execution with Nextflow Tower using this url https://staging-tower.xyz/watch/TWl5sx6tUPCiZ",
            "[82/a45d22] Submitted process > RNASEQ:FASTQC (FASTQC on ggal_liver)",
            "[07/d3280f] Submitted process > RNASEQ:INDEX (ggal_1_48850000_49020000)",
            "[4a/3f5108] Submitted process > RNASEQ:FASTQC (FASTQC on ggal_gut)",
            "[21/c84367] Submitted process > RNASEQ:QUANT (ggal_liver)",
            "[9a/07ec7b] Submitted process > RNASEQ:QUANT (ggal_gut)",
            "[2f/08de3e] Submitted process > MULTIQC",
            "Done! Open the following report in your browser --> results/multiqc_report.html"
        ],
        "rewindToken": "b/35796553717042515986933654452443998602419396311452155904",
        "forwardToken": "f/35796559572393077548729658682759299686869043613106765824",
        "pending": false,
        "message": null,
        "downloads": [
            {
                "fileName": "nf-TWl5sx6tUPCiZ.txt",
                "displayText": "Nextflow console output",
                "saveName": "nf-TWl5sx6tUPCiZ.txt"
            },
            {
                "fileName": "nf-TWl5sx6tUPCiZ.log",
                "displayText": "Nextflow log file",
                "saveName": "nf-TWl5sx6tUPCiZ.log"
            }
        ],
        "truncated": false
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

## Describe workflow task log
Retrieves a workflow entity's task log with a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/log/{taskId}
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |
| `taskId` | `int` | query | **Required**. Workflow task id |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/log/{taskId}'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "log": {
        "entries": [
            "nxf-scratch-dir ip-172-31-2-41.eu-west-1.compute.internal:/tmp/nxf.G5jZ30Jn9m"
        ],
        "rewindToken": "b/35796556826212411566073133013331948128190040440621432832",
        "forwardToken": "f/35796556826212411566073133013331948128190040440621432832",
        "pending": false,
        "message": null,
        "downloads": [
            {
                "fileName": ".command.out",
                "displayText": "Task stdout",
                "saveName": "task-2.command.out.txt"
            },
            {
                "fileName": ".command.err",
                "displayText": "Task stderr",
                "saveName": "task-2.command.err.txt"
            },
            {
                "fileName": ".command.log",
                "displayText": "Task log file",
                "saveName": "task-2.command.log.txt"
            }
        ],
        "truncated": false
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

## Download workflow
Download a workflow entity with a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/download?fileName=:fileName
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |
| `fileName` | `string` | query | **Required**. File to download in the format `nf-{workflowid}.txt` or `nf-{workflowid}.log` |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/download?fileName=nf-{:workflowId}.{txt,log}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight text >}}
N E X T F L O W  ~  version 20.10.0
Pulling nextflow-io/rnaseq-nf ...
 downloaded from https://github.com/nextflow-io/rnaseq-nf.git
Launching `nextflow-io/rnaseq-nf` [happy_laplace] - revision: 708cf52a33 [master]
 R N A S E Q - N F   P I P E L I N E
 ===================================
 transcriptome: /.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa
 reads        : /.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/*_{1,2}.fq
 outdir       : results
 
Monitor the execution with Nextflow Tower using this url https://staging-tower.xyz/watch/3V5QcabVRPtuSM
[aa/f4e895] Submitted process > RNASEQ:INDEX (ggal_1_48850000_49020000)
[dc/8bc8f5] Submitted process > RNASEQ:FASTQC (FASTQC on ggal_liver)
[7e/7af0e3] Submitted process > RNASEQ:FASTQC (FASTQC on ggal_gut)
[ed/b6fa73] Submitted process > RNASEQ:QUANT (ggal_liver)
[a7/a2fb58] Submitted process > RNASEQ:QUANT (ggal_gut)
[76/3f1a05] Submitted process > MULTIQC

Done! Open the following report in your browser --> results/multiqc_report.html

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

## Download workflow task
Download a workflow entity's task with a given ID.

{{% tip %}}
**GET** /api/workflow/:workflowId/download/:taskId?fileName=:fileName
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |
| `taskId` | `string` | query | **Required**. Workflow task entity id |
| `fileName` | `string` | query | **Required**. File to download in the format `.command.log` or `.command.err`. `.command.out` |

### Code sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X GET 'localhost:8000/api/workflow/{workflowId}/download?fileName={fileName}'
{{< /highlight >}}

### Default response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight text >}}
Threads = 1
Vertex length = 31
Hash functions = 5
Filter size = 4194304
Capacity = 2
Files: 
index/ref_k31_fixed.fa
--------------------------------------------------------------------------------
Round 0, 0:4194304
Pass	Filling	Filtering
1	0	0	
2	0	0
True junctions count = 7
False junctions count = 476
Hash table size = 483
Candidate marks count = 491
--------------------------------------------------------------------------------
Reallocating bifurcations time: 0
True marks count: 16
Edges construction time: 0
--------------------------------------------------------------------------------
Distinct junctions = 7

for info, total work write each  : 2.331    total work inram from level 3 : 4.322  total work raw : 25.000 
Bitarray          901696  bits (100.00 %)   (array + ranks )
final hash             0  bits (0.00 %) (nb in final hash 0)

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

## Launch workflow
Create and launch a workflow.

{{% tip %}}
**POST** /api/workflow/launch
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `computeEnvId` | `string` | body | **Required**. The compute environment id where the execution will be launched. |
| `pipeline` | `string` | body | **Required**. Pipeline name or a Git repository URL. |
| `workDir` | `string` | body | The bucket path where the pipeline scratch data is stored. |
| `revision` | `string` | body | A valid repository commit Id, tag or branch name. |
| `sessionId` | `string` | body | Arbitrary tracking session id. |
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
  "launch": {
    "computeEnvId": "6HkkIgaYT7iqwO7V0n6aWa",
    "pipeline": "https://github.com/nextflow-io/rnaseq-nf",
    "workDir": "s3://random-tower-bucket",
    "revision": "master",
    "sessionId": "CmMDhg",
    "configProfiles": [
      "docker"
    ],
    "configText": "{\"params\":{\"param1\":\"value\", \"param2\":\"value\"}}",
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

### Code Sample
{{< highlight bash >}}
curl -H 'Authorization: Bearer {access_token}' \
     -X POST 'localhost:8000/api/workflow/{workflowId}/launch'
     -d '{"key" : "value"}'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

{{< highlight json >}}
{
    "workflowId": "3V5QcabVRPtuSM"
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

## Cancel workflow
Cancel the workflow entity with a given ID

{{% tip %}}
**POST** /api/workflow/:workflowId/cancel
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code Sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X POST 'localhost:8000/api/workflow/{workflowId}/cancel'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 200 OK
{{< /highlight >}}

### Bad response 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

{{< highlight json >}}
{
    "message": "Invalid cancel request for workflow id: 5exbg5o9ptqq34; status: CANCELLED"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}

## Delete workflow

Delete the workflow entity with a given ID

{{% tip %}}
**DELETE** /api/workflow/:workflowId
{{% /tip %}}

### Request parameters
| Name | Type     | In | Description           |
|------|----------|----|-----------------------|
| `workflowId` | `string` | query | **Required**. Workflow entity id |

### Code Sample
{{< highlight bash >}}
curl -H "Content-Type: application/json" \
     -H 'Authorization: Bearer {access_token}' \
     -X DELETE 'localhost:8000/api/workflow/{workflowId}/metrics'
{{< /highlight >}}

### Standard response 
{{< highlight bash >}}
Status: 204 No Content
{{< /highlight >}}

### Bad response 
{{< highlight bash >}}
Status: 400 Bad Request
{{< /highlight >}}

{{< highlight json >}}
{
    "message": "Invalid cancel request for workflow id: 5exbg5o9ptqq34; status: CANCELLED"
}
{{< /highlight >}}

### Forbidden 
{{< highlight bash >}}
Status: 403 Forbidden
{{< /highlight >}}
