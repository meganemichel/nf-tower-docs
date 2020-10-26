---
title: Application programming interface
aliases:
- "/docs/api"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"
headline: "Tower - an API centric platform"
description: 'Automating pipeline execution through webhooks'
menu:
  docs:
    parent: API
    weight: 1


---

Tower is API centric, it exposes a public API with all necessary calls to manage and monitor Nextflow workflows programatically. This allows organisations to extend their existing solutions by simply leveraging the Tower API.

### Authorisation token

Authorisation tokens can be found on your settings on the top right of the page. Select `Your tokens` and click on the `Show HTTP token` button.

{{% pretty_screenshot img="/uploads/2020/10/api_tokens.png" %}}


### Example call using the cURL command

```curl -H "Authorization: Basic QHRva2XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXWM4MGU4MjE5MDM=" https://tower.nf/api/workflow```

where `QHRva2XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXWM4MGU4MjE5MDM=` is authorisation token shown in the screenshot above.  

{{% pretty_screenshot img="/uploads/2020/10/api_example_call.png" %}}

### API documentation

Detailed API documentation can be found in our [API page](https://tower.nf/openapi/index.html)
