---
title: "mc ping — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ping.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ping"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ping — SILO (MinIO Object Storage)"
---
# mc ping

## Syntax

The [`mc ping`](#command-mc.ping) command performs a liveness check on a specified target.

EXAMPLE

The following sends a response request to the target(s) and outputs the minimum, maximum, average, and roundtrip times of the response, as well as the number of errors encountered when processing the request.

```
mc ping play --count 5
```

The command pings the deployment at the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] `play` for five cycles.
The output resembles the following:

```
1: https://play.min.io   min=213.00ms   max=213.00ms   average=213.00ms   errors=0   roundtrip=213.00ms
2: https://play.min.io   min=67.15ms    max=213.00ms   average=140.07ms   errors=0   roundtrip=67.15ms
3: https://play.min.io   min=67.15ms    max=213.00ms   average=115.85ms   errors=0   roundtrip=67.41ms
4: https://play.min.io   min=61.26ms    max=213.00ms   average=102.20ms   errors=0   roundtrip=61.26ms
5: https://play.min.io   min=61.26ms    max=213.00ms   average=95.03ms    errors=0   roundtrip=66.36ms
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ping                       \
                 TARGET                     \
                 [--count, -c value]        \
                 [--error-count, -e value]  \
                 [--interval, -i value]     \
                 [--distributed, -a value]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.ping.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or prefix where the command should run.

--count[](#mc.ping.-count)
Optional

Specify the number of times to perform the check.

If not specified, the liveness check performs continuously until stopped.

--error-count[](#mc.ping.-error-count)
Optional

Specify a number of errors to receive before exiting.

For example, to stop the ping process after receiving five errors, use

```
mc ping TARGET -e 5
```

--exit[](#mc.ping.-exit)
Optional

New in version RELEASE.2023-05-30T22-41-38Z.

Exit after the first successful check.

--interval[](#mc.ping.-interval)
Optional

The length of time in seconds to wait between requests.

By default, the command waits 1 second between requests.

--distributed[](#mc.ping.-distributed)
Optional

Send requests to all servers in the MinIO cluster.

Note

Use this option for distributed deployments where you have direct access to each node or pod.
This flag does not work when nodes are placed behind a service, such as a load balancer.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Return Latency and Liveness for 5 Requests

The following command sends a liveness check for a deployment with the alias `myminio` five times, outputs the result of each check, then ends.

```
mc ping myminio --count 5
```

### Send Liveness Checks Repeatedly with 5 Minute Wait Between Each Request

The following command sends continuous liveness check requests with an interval of 5 minutes (300 seconds) between each request.

```
mc ping myminio --interval 300
```

### End Liveness Checks for Error Counts Greater Than 20

The following command sends continuous liveness checks until 20 errors have been encountered:

```
mc ping myminio --error-count 20
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).