---
title: "mox helm repository"
excerpt: "mox helm repository homepage"
permalink: /helm/
redirect_from:
  - /charts/
date: 2020-04-17T23:54:20+02:00
last_modified_at: 2020-08-03T03:11:31+02:00
toc: true
toc_label: "Content"
toc_sticky: true
categories:
  - sysadmin
tags:
  - helm
  - kubernetes
---

![Lint and Test Charts](https://github.com/javimox/helm-charts/workflows/Lint%20and%20Test%20Charts/badge.svg) [![](https://github.com/javimox/helm-charts/workflows/Release%20Charts/badge.svg?branch=master)](https://github.com/javimox/helm-charts/actions) ![Latest Stable Versions](https://github.com/javimox/helm-charts/workflows/Latest%20Stable%20Versions/badge.svg)

[*helm'd*](https://github.com/helm/helm) applications ready to be launched on [Kubernetes](https://kubernetes.io/).  
Have fun!

## TL;DR

```bash
$ helm repo add mox https://helm.mox.sh
$ helm search repo mox
$ helm install my-release mox/<chart>
```

## Project Status

`master` supports Helm 3 only, i. e. both `v1` and `v2` [API version](https://helm.sh/docs/topics/charts/#the-apiversion-field) charts are installable.

Helm Charts of `mox` repository are available on:
 * [helm.mox.sh](https://helm.mox.sh)
 * [hub.helm.sh](https://hub.helm.sh/charts/mox)
 * [artifacthub.io](https://artifacthub.io/packages/search?repo=mox)
 * [hub.kubeapps.com](https://hub.kubeapps.com/charts/mox)
 
If you find any errors, feel free to open an issue on [my GitHub](https://github.com/javimox/helm-charts/tree/master).

## Chart Sources

* `charts/confluence-server`: Atlassian Confluence Server chart @ [helm.mox.sh](https://mox.sh/helm/charts/confluence-server/)
* `charts/crowd`: Atlassian Crowd chart @ [helm.mox.sh](https://mox.sh/helm/charts/crowd/)
* `charts/jira-software`: Jira Software chart @ [helm.mox.sh](https://mox.sh/helm/charts/jira-software/)

## Do you want to add your charts? Feel free to:

* Fork this repository
* Create a branch off master named after the chart
* Create a PR

## About Helm

* [Quick Start guide](https://helm.sh/docs/intro/quickstart/)
* [Using Helm Guide](https://helm.sh/docs/intro/using_helm/)
