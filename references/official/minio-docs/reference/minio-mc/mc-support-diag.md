---
title: "mc support diag — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-diag.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-diag"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support diag — SILO (MinIO Object Storage)"
---
# mc support diag

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

Command History

The command used to create the diagnostic report has changed over time.

MinIO Client Release

Command

Notes

RELEASE.2022-02-13T23-26-13Z

`mc support diag`

Command moved to `mc support`

RELEASE.2020-11-17T00-39-14Z

`mc admin subnet health`

Command made a SUBNET subcommand

RELEASE.2020-10-03T02-54-56Z

`mc admin health`

Command renamed to health

Original Command

`mc admin obd`

Command renamed `mc admin health`

## Description

The [`mc support diag`](#command-mc.support.diag) command generates a health report for a MinIO deployment.

For deployments registered with the MinIO subscription network ([SUBNET](https://min.io/pricing?jmp=docs)), the command generates and uploads the health report for analysis.
Optionally, automate generating and uploading the report every 24 hours by enabling [[reference/minio-mc/mc-support-callhome#command-mc.support.callhome|`callhome`]].

The resulting health report is intended for use by MinIO Engineering via SUBNET and may contain internal or private data points.
Exercise caution before sending a health report to a third party or posting the health report in a public forum.

MinIO recommends that you run the health diagnostics when first provisioning the cluster and again at any failure scenario.

Use the [`mc support diag`](#command-mc.support.diag) command to trigger the diagnostic test.
For clusters registered with SUBNET, the command uploads the results as part of SUBNET Health reports.

For airgapped or firewalled environments, or other environments that prevent direct network access from the deployment, you can save the report locally with the [`--airgap`](#mc.support.diag.-airgap) flag.
After saving, you can then upload the results of the test to SUBNET manually.

### Sample Output

```
● CPU Info ... ✔
● Disk Info ... ✔
● Net Info ... ✔
● Os Info ... ✔
● Mem Info ... ✔
● Process Info ... ✔
● Server Config ... ✔
● System Errors ... ✔
● System Services ... ✔
● System Config ... ✔
● Admin Info ... ✔
*********************************************************************************
                                WARNING!!
     ** THIS FILE MAY CONTAIN SENSITIVE INFORMATION ABOUT YOUR ENVIRONMENT **
     ** PLEASE INSPECT CONTENTS BEFORE SHARING IT ON ANY PUBLIC FORUM **
*********************************************************************************
mc: MinIO diagnostics report saved to myminio-health_20231111053323.json.gz
```

The gzipped output contains the requested health information.

## Examples

### Generate Health Data for a Cluster and Automatically Upload to SUBNET

Generate health data for a MinIO cluster and automatically for a MinIO cluster at alias `minio1` for transmission to SUBNET.

```
mc support diag minio1
```

The automatic upload of data only occurs for deployments under a Commerical License.

### Generate Health Data for a Cluster to Upload Manually

Generate a diagnostic report for a MinIO deployment at alias `minio2` and save it for manual upload to SUBNET:

```
mc support diag minio2 --airgap
```

Run the command to download the `.gzip` file

Login to [https://subnet.min.io](https://subnet.min.io) and select the Deployments section

Select the deployment for the report

Select the Upload button

Drag and drop the file or browse to the `.gzip` file location to upload it

### Upload Data to SUBNET with Strict Anonymization

Generates health data for a MinIO cluster at alias `myminio` and anonymizes all sensitive data, including host names.

```
mc support diag myminio --anonymize=strict
```

## Syntax

The command has the following syntax:

```
mc [GLOBALFLAGS] support diag                   \
                         ALIAS                  \
                         [--airgap]             \
                         [--anonymize=<string>] \
                         [--api-key string]
```

### Parameters

ALIAS[](#mc.support.diag.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

--airgap[](#mc.support.diag.-airgap)
Optional

Use in environments without network access to SUBNET (for example, airgapped, firewalled, or similar configuration).
Generates the diagnostic report and saves it to the location where you ran the command.

You must manually upload the report to SUBNET.

For instructions, see the [airgap example](#minio-support-diagnostics-airgap).

If the deployment is airgapped, but the local device where you are using the [[reference/minio-mc#minio-client|minio client]] has network access, you do not need to use the `--airgap` flag.

--anonymize[](#mc.support.diag.-anonymize)

New in version mc: RELEASE.2023-11-10T21-37-17Z

MinIO anonymizes data loaded to SUBNET.
Beginning with mc `RELEASE.2023-11-10T21-37-17Z`, MinIO does not anonymize host names.
This is the default `standard` anonymization mode.

Valid values are `=strict` or `=standard`.

To anonymize all data, including host names, pass this parameter with the `strict` mode.

```
mc support diag minio --anonymize=strict
```

--api-key[](#mc.support.diag.-api-key)
Optional

Takes the account’s API key value from SUBNET.

This value is only required for airgapped environments where MinIO has not already stored the API key for the deployment.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).