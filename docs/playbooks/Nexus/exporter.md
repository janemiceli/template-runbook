---
layout: default
---
gcd-nexus-exporter
========================

The metrics exposed by Nexus ([https://nexus.hpcwp.com/service/metrics/data](https://nexus.hpcwp.com/service/metrics/data)) are not in a format that Prometheus can read. Therefore, it is necessary to "translate" the data so that Prometheus can ingest it. To accomplish this, Nexus is deployed in a Kubernetes pod with two containers:

* One container for the Nexus itself
* Another "sidecar" container named `gcd-nexus-exporter`

The `gcd-nexus-exporter` periodically scrapes `/service/metrics/data` and then serves the translated data on an HTTP `/metrics` endpoint on port 7979 for Prometheus to scrape.

The `gcd-nexus-exporter` is built from its [repo](https://github.azc.ext.hp.com/cwp/gcd-nexus-exporter) and then [pushed](https://github.azc.ext.hp.com/cwp/gcd-quantum/blob/master/docs/playbooks/ECR/index.md#test-push) into the existing ECR repo 871386769552.dkr.ecr.us-west-2.amazonaws.com/cwp/gcd-nexus-exporter (in the DEV AWS account).
