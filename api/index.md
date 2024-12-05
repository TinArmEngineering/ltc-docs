---
title: API
layout: default
nav_order: 20
---

## Get example job

Fetches an example job and outputs its json representation.  This is an open call that requires no API key.  It is these examples that are used as the basis for new jobs in the web GUI.

```bash
# An electromagnetic simulation of an interior mounted V rotor and 
# concentrated wound tooth segmented stator.
curl https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg
```

```bash
# A thermal simulation of a distributed hairpin stator
curl https://api.ltc.tinarmengineering.com/jobs/thermal_none_dwpss
```

```bash
# A mechanical simulation of a surface mounted breadload rotor
curl https://api.ltc.tinarmengineering.com/jobs/mechanical_spmbrl_none
```

## Create new job from example job

Fetches and saves an example job to file.  Posts example job as a new job

```bash
curl -o example.json https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg
curl -X POST https://api.ltc.tinarmengineering.com/jobs?apikey=my-api-key \
     -d @example.json \
     -H "Content-Type: application/json"
```

## Retrieve a job

Retrieves an existing job by its ```"id"``` field
```bash
curl https://api.ltc.tinarmengineering.com/jobs/my-job-id?apikey=my-api-key
```

