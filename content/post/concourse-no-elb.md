---
authors:
- cunnie
categories:
- BOSH
- Concourse
date: 2016-08-14T12:57:07-07:00
draft: true
short: |
  nginx is a less-expensive alternative to a load balancer for a BOSH-deployed Concourse's SSL termination.
title: Concourse without a Load Balancer
---

## Abstract

[Concourse](http://concourse.ci/) is a continuous integration (CI) server.
It can be deployed manually or via [BOSH](http://concourse.ci/clusters-with-bosh.html).

In this blog post, we will discuss modifying the manifest of a BOSH-deployed
Concourse CI server to natively accept SSL connections (no load balancer). This
may reduce the complexity and cost <sup>[[ELB-pricing]](#ELB-pricing)</sup> of the Concourse deployment.

### 0. Pre-requisites

[Deploy Concourse with BOSH](http://concourse.ci/clusters-with-bosh.html)
<sup>[[prereqs]](#prereqs)</sup>.

### 1. Using Concourse's Built-in TLS

Concourse has BOSH job properties that allow you to set the TLS key and
certificate:

```yaml
instance_groups:
  jobs:
    atc:
      properties:
        tls_cert:
          -----BEGIN CERTIFICATE-----
          MIIFXDCCBESgAwIBAgIQOvRHkhKyb/k9O4xvIi9zZTANBgkqhkiG9w0BAQsFADCB
          ...
          NaaNSyS8pHUJhaq+ZiC7zM2YsuLBICPQfsunHGrho4k=
          -----END CERTIFICATE-----
          -----BEGIN CERTIFICATE-----
          MIIGCDCCA/CgAwIBAgIQKy5u6tl1NmwUim7bo3yMBzANBgkqhkiG9w0BAQwFADCB
          ....
          pu/xO28QOG8=
          -----END CERTIFICATE-----
        tls_key: |
          -----BEGIN RSA PRIVATE KEY-----
```

This is a very simple solution whose only downside is the inabilll

## Footnotes

<a name="ELB-pricing"><sup>[ELB-pricing]</sup></a> ELB pricing, as of this
writing, is
[$0.025/hour](https://aws.amazon.com/elasticloadbalancing/pricing/), $0.60/day,
$219.1455 / year (assuming 365.2425 days / year).

<a name="prereqs"><sup>[prereqs]</sup></a>
Note the following:

- We are deploying to [Google Cloud Platform]()'s [Google Compute Engine]()
  (GCE), and thus are using a GCE stemcell. If deploying to a different IaaS
  (e.g. [Amazon Web Services]() (AWS), choose the appropriate stemcell from
  [bosh.io](http://bosh.io/stemcells))

-  We are using an experimental Golang-based
  [BOSH command line
  interface](https://github.com/cloudfoundry/bosh-init/tree/cli) (CLI), and the
  arguments are slightly different from those of canonical ruby-based
  [BOSH CLI](https://github.com/cloudfoundry/bosh/tree/master/bosh_cli); however, the arguments are similar enough to be readily adapted to the ruby CLI (e.g. the Golang CLI's `bosh upload-stemcell` equivalent in the Ruby CLI is `bosh upload stemcell` (no dash)).

  ```bash
  bosh upload-stemcell https://storage.googleapis.com/bosh-cpi-artifacts/light-bosh-stemcell-3262.5-google-kvm-ubuntu-trusty-go_agent.tgz
  bosh upload-release http://bosh.io/d/github.com/cloudfoundry-incubator/garden-runc-release?v=0.5.0
  bosh upload-release http://bosh.io/d/github.com/concourse/concourse?v=1.6.0
  bosh upload-release https://github.com/cloudfoundry-community/nginx-release/releases/download/v4/nginx-4.tgz
  ```

{{< responsive-figure src="/images/pairing.jpg" class="right small" >}}

{{< responsive-figure src="/images/pairing.jpg" class="left" >}}

{{< responsive-figure src="/images/pairing.jpg" >}}
