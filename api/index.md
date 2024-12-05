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

## Prerequisites

These examples are written in bash script and require the command line tools curl and jq.  These are often pre-installed or easy to install through your local package manager.

- [curl](https://curl.se/){:target="_blank"} for transferring data with URLs
- [jq](https://jqlang.github.io/jq/download/){:target="_blank"} for parsing JSON responses

## Get example job

Fetches an example job and outputs its JSON representation.  This is an open call that requires no API key.  It is these examples that are used as the basis for new jobs in the web GUI.

Example jobs are viewable in a browser:
[https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg](https://api.ltc.tinarmengineering.com/jobs/electromagnetic_ipmv_fscwseg){:target="_blank"}

API calls used:

[```GET /jobs/{id}```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}

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

[```GET /jobs/{id}```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}
[```POST /jobs```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/post_jobs){:target="_blank"}

```bash
api_root=https://api.ltc.tinarmengineering.com
api_key=my-api-key
output_file=example.json

# Grab an example job and save it to file
curl -o $output_file $api_root/jobs/electromagnetic_ipmv_fscwseg

# Post the example JSON as a new job.
# Responds with the new job's JSON representation
curl -X POST $api_root/jobs?apikey=$api_key \
     -d @$output_file \
     -H "Content-Type: application/json"
```

## Retrieve job

Creates a job then retrieves it by its ```"id"``` field.

API calls used:

[```GET /jobs/{id}```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}
[```POST /jobs```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/post_jobs){:target="_blank"}


```bash
api_root=https://api.ltc.tinarmengineering.com
api_key=my-api-key
output_file=example.json

# Grab an example job and save it to file
curl -o $output_file $api_root/jobs/electromagnetic_ipmv_fscwseg

# Post the example JSON as a new job.
# Save its id to the job_id variable
job_id=$(curl -sX POST $api_root/jobs?apikey=$api_key \
              -d @$output_file \
              -H "Content-Type: application/json" | jq -r '.id')

# Retrieve the job using its stored id
curl $api_root/jobs/$job_id?apikey=$api_key
```

## Queue job for solving

Creates a job, queues it for solving and monitors its status.

**Polling is not good.** Please don't be tempted to use polling, it is not a good practice. There are examples of how to use web sockets to monitor status using the [python client](../py-client)

API calls used:

[```GET /jobs/{id}```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/get_jobs__id_){:target="_blank"}
[```POST /jobs```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/post_jobs){:target="_blank"}
[```PUT /jobs/{id}/status/{status}```](https://api.ltc.tinarmengineering.com/docs/index.html#/jobs/put_jobs__id__status__status_){:target="_blank"}


```bash
api_root=https://api.ltc.tinarmengineering.com
api_key=my-api-key
output_file=example.json

# Grab an example job and save it to file
curl -o $output_file $api_root/jobs/electromagnetic_ipmv_fscwseg

# Post the example JSON as a new job.
# Save its id to the job_id variable
job_id=$(curl -sX POST $api_root/jobs?apikey=$api_key \
              -d @$output_file \
              -H "Content-Type: application/json" | jq -r '.id')

# Queue the new job for solving
curl -X PUT $api_root/jobs/$job_id/status/10?apikey=$api_key

# Start monitoring the job status
echo "Monitoring job $job_id status..."

# Loop until the job status is either "completed" or "quarantined"
while true; do
  status=$(curl -s $api_root/jobs/$job_id?apikey=$api_key | jq -r '.status')
  echo "Current status: $status"

   # Check if the job has finished (completed or quarantined)
  if [ $status == 70 ] || [ $status == 80 ]; then
    echo "Job $job_id has finished with status: $status"
    break
  fi
  sleep 10
done
```