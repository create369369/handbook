# Cody Analytics infrastructure operations

<!--
Generated documentation; DO NOT EDIT. Regenerate using this command: 'sg msp operations generate-handbook-pages'

Last updated: 2024-01-30 00:36:07.182855 +0000 UTC
Generated from: https://github.com/sourcegraph/managed-services/tree/5c3790bfdd225d7ce3ccf5dd8818a3f58d85aba2
-->

This document describes operational guidance for Cody Analytics infrastructure.
This service is operated on the [Managed Services Platform (MSP)](../teams/core-services/managed-services/platform.md).

If you need assistance with MSP infrastructure, reach out to the [Core Services](../teams/core-services/index.md) team in #discuss-core-services.

## Service overview

| PROPERTY     | DETAILS                                                                                                            |
| ------------ | ------------------------------------------------------------------------------------------------------------------ |
| Service ID   | [`cody-analytics`](https://github.com/sourcegraph/managed-services/blob/main/services/cody-analytics/service.yaml) |
| Owners       | **cody-strat**                                                                                                     |
| Service kind | Cloud Run service                                                                                                  |
| Environments | [dev](#dev)                                                                                                        |
| Docker image | `us-central1-docker.pkg.dev/sourcegraph-dev/cody-analytics/service`                                                |
| Source code  | [`github.com/sourcegraph/cody-analytics` - `.`](https://github.com/sourcegraph/cody-analytics/tree/HEAD/.)         |

## Environments

### dev

| PROPERTY       | DETAILS                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------------ |
| Project ID     | [`cody-analytics-dev-bd34`](https://console.cloud.google.com/run?project=cody-analytics-dev-bd34)      |
| Category       | **test**                                                                                               |
| Resources      |                                                                                                        |
| Alerts         | [GCP monitoring](https://console.cloud.google.com/monitoring/alerting?project=cody-analytics-dev-bd34) |
| Sentry         | [`cody-analytics-dev`](https://sourcegraph.sentry.io/projects/cody-analytics-dev/)                     |
| Domain         | [cody-analytics.sgdev.org](https://cody-analytics.sgdev.org)                                           |
| Cloudflare WAF | ✅                                                                                                     |

MSP infrastructure access needs to be requested using Entitle for time-bound privileges. Test environments may have less stringent requirements.

| ACCESS                   | ENTITLE REQUEST TEMPLATE                                                                                                                                                                                                                                                                                                                                  |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GCP project read access  | [Entitle request for the 'Engineering Projects' folder](https://app.entitle.io/request?data=eyJkdXJhdGlvbiI6IjIxNjAwIiwianVzdGlmaWNhdGlvbiI6IkVOVEVSIEpVU1RJRklDQVRJT04gSEVSRSIsInJvbGVJZHMiOlt7ImlkIjoiZGY3NWJkNWMtYmUxOC00MjhmLWEzNjYtYzlhYTU1MGIwODIzIiwidGhyb3VnaCI6ImRmNzViZDVjLWJlMTgtNDI4Zi1hMzY2LWM5YWE1NTBiMDgyMyIsInR5cGUiOiJyb2xlIn1dfQ%3D%3D) |
| GCP project write access | [Entitle request for the 'Engineering Projects' folder](https://app.entitle.io/request?data=eyJkdXJhdGlvbiI6IjIxNjAwIiwianVzdGlmaWNhdGlvbiI6IkVOVEVSIEpVU1RJRklDQVRJT04gSEVSRSIsInJvbGVJZHMiOlt7ImlkIjoiYzJkMTUwOGEtMGQ0ZS00MjA1LWFiZWUtOGY1ODg1ZGY3ZDE4IiwidGhyb3VnaCI6ImMyZDE1MDhhLTBkNGUtNDIwNS1hYmVlLThmNTg4NWRmN2QxOCIsInR5cGUiOiJyb2xlIn1dfQ%3D%3D) |

For Terraform Cloud access, see [dev Terraform Cloud](#dev-terraform-cloud).

#### dev Cloud Run

The Cody Analytics dev service implementation is deployed on [Google Cloud Run](https://cloud.google.com/run).

| PROPERTY     | DETAILS                                                                                                                                                                                                                                                                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Console      | [Cloud Run service](https://console.cloud.google.com/run?project=cody-analytics-dev-bd34)                                                                                                                                                                                                                                            |
| Service logs | [GCP logging](https://console.cloud.google.com/logs/query;query=resource.type%20%3D%20%22cloud_run_revision%22%20-logName%3D~%22logs%2Frun.googleapis.com%252Frequests%22;summaryFields=jsonPayload%252FInstrumentationScope,jsonPayload%252FBody,jsonPayload%252FAttributes%252Ferror:false:32:end?project=cody-analytics-dev-bd34) |

You can also use `sg msp` to quickly open a link to your service logs:

```bash
sg msp logs cody-analytics dev
```

#### dev Terraform Cloud

This service's configuration is defined in [`sourcegraph/managed-services/services/cody-analytics/service.yaml`](https://github.com/sourcegraph/managed-services/blob/main/services/cody-analytics/service.yaml), and `sg msp generate cody-analytics dev` generates the required infrastructure configuration for this environment in Terraform.
Terraform Cloud (TFC) workspaces specific to each service then provisions the required infrastructure from this configuration.
You may want to check your service environment's TFC workspaces if a Terraform apply fails (reported via GitHub commit status checks in the [`sourcegraph/managed-services`](https://github.com/sourcegraph/managed-services) repository, or in #alerts-msp-tfc).

> [!NOTE]
> If you are looking for service logs, see the [dev Cloud Run](#dev-cloud-run) section instead. In general:
>
> - check service logs ([dev Cloud Run](#dev-cloud-run)) if your service has gone down or is misbehaving
> - check TFC workspaces for infrastructure provisioning or configuration issues

To access this environment's Terraform Cloud workspaces, you will need to [log in to Terraform Cloud](https://app.terraform.io/app/sourcegraph) and then [request Entitle access to membership in the "Managed Services Platform Operator" TFC team](https://app.entitle.io/request?data=eyJkdXJhdGlvbiI6IjM2MDAiLCJqdXN0aWZpY2F0aW9uIjoiSlVTVElGSUNBVElPTiBIRVJFIiwicm9sZUlkcyI6W3siaWQiOiJiMzg3MzJjYy04OTUyLTQ2Y2QtYmIxZS1lZjI2ODUwNzIyNmIiLCJ0aHJvdWdoIjoiYjM4NzMyY2MtODk1Mi00NmNkLWJiMWUtZWYyNjg1MDcyMjZiIiwidHlwZSI6InJvbGUifV19).
The "Managed Services Platform Operator" team has access to all MSP TFC workspaces.

> [!WARNING]
> You **must [log in to Terraform Cloud](https://app.terraform.io/app/sourcegraph) before making your Entitle request**.
> If you make your Entitle request, then log in, you will be removed from any team memberships granted through Entitle by Terraform Cloud's SSO implementation.

The Terraform Cloud workspaces for this service environment are [grouped under the `msp-cody-analytics-dev` tag](https://app.terraform.io/app/sourcegraph/workspaces?tag=msp-cody-analytics-dev), or you can use:

```bash
sg msp tfc view cody-analytics dev
```
