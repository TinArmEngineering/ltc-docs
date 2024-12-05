---
title: API
layout: default
nav_order: 20
---

## Get example job

```bash
curl -o example.json https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg
```

## Create new job from example job

```bash
curl -X POST https://api.ltc.tinarmengineering.com/jobs?apikey=my-api-key \
     -d @em_ipmv_fscwseg_example.json \
     -H "Content-Type: application/json"
```

## Retrieve a job by its ID

```bash
curl https://api.ltc.tinarmengineering.com/jobs/my-job-id?apikey=my-api-key
```
