---
title: API
layout: default
nav_order: 20
---

# API
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Get example job

Fetches an example job and outputs its json representation.  This is an open call that requires no API key.  It is these examples that are used as the basis for new jobs in the web GUI.

Example jobs are viewable in a browser:
[https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg](https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg){:target="_blank"}

API calls used:

- [GET /jobs/{id}](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}

```bash
# Setup our API root for the following calls
api_root=https://api.ltc.tinarmengineering.com

# An electromagnetic simulation of an interior mounted V rotor and 
# concentrated wound tooth segmented stator.
curl $api_root/jobs/electromagnetic_ipmv_fscwseg

# A thermal simulation of a distributed hairpin stator
curl $api_root/jobs/thermal_none_dwpss

# A mechanical simulation of a surface mounted breadload rotor
curl $api_root/jobs/mechanical_spmbrl_none
```

## Create new job from example job

Fetches and saves an example job to file.  Posts example job as a new job

API calls used:

- [GET /jobs/{id}](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}
- [POST /jobs](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/post_jobs){:target="_blank"}

```bash
api_root=https://api.ltc.tinarmengineering.com
api_key=my-api-key
output_file=example.json

# Grab an example job and save it to file
curl -o $output_file $api_root/jobs/electromagnetic_ipmv_fscwseg

# Post the example json as a new job.
# Responds with the new job's json representation
curl -X POST $api_root/jobs?apikey=$api_key \
     -d @$output_file \
     -H "Content-Type: application/json"
```

## Retrieve a job

Retrieves an existing job by its ```"id"``` field

API calls used:

- [GET /jobs/{id}](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}

```bash
api_root=https://api.ltc.tinarmengineering.com
api_key=my-api-key

curl $api_root/jobs/my-job-id?apikey=$api_key
```

