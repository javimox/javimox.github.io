---
title: "mox helm repository"
excerpt: "mox helm repository homepage"
permalink: /helm/
redirect_from:
  - /charts/
date: 2020-04-17T23:54:20+02:00
last_modified_at: 2020-08-30T12:09:23+02:00
toc: true
toc_label: "Content"
toc_sticky: true
categories:
  - sysadmin
tags:
  - helm
  - kubernetes
---

[![Downloads](https://img.shields.io/github/downloads/javimox/helm-charts/total?color=blue&label=Downloads&style=plastic)](https://github.com/javimox/helm-charts/releases/)  
![Lint and Test Charts](https://github.com/javimox/helm-charts/workflows/Lint%20and%20Test%20Charts/badge.svg)  
[![](https://github.com/javimox/helm-charts/workflows/Release%20Charts/badge.svg?branch=master)](https://github.com/javimox/helm-charts/actions)  
![Latest Stable Versions](https://github.com/javimox/helm-charts/workflows/Latest%20Stable%20Versions/badge.svg)

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
 * [artifacthub.io](https://artifacthub.io/packages/search?repo=mox)
 * [hub.kubeapps.com](https://hub.kubeapps.com/charts/mox)
 
If you find any errors, feel free to open an issue on [my GitHub](https://github.com/javimox/helm-charts/tree/master).

## Chart Sources

* `[charts/confluence-server]`: Atlassian Confluence Server chart. [mox.sh](https://mox.sh/helm/charts/confluence-server/) [github.com](https://github.com/javimox/helm-charts/tree/master/charts/confluence-server)
* `[charts/crowd]`: Atlassian Crowd chart. [mox.sh](https://mox.sh/helm/charts/crowd/) [github.com](https://github.com/javimox/helm-charts/tree/master/charts/crowd)
* `[charts/jira-software]`: Jira Software chart. [mox.sh](https://mox.sh/helm/charts/jira-software/) [github.com](https://github.com/javimox/helm-charts/tree/master/charts/jira-software)

## Contribute

Contributions are welcome! If you have any issue deploying these charts or a enhancement feature, feel free to open an issue on [my GitHub](https://github.com/javimox/helm-charts/tree/master).

If you want to add your Charts to the **mox** repository or modify those that are available, you are welcome to:

* Fork this repository
* Create a branch off master named after your chart
* Create a PR

## About Helm

* [Quick Start guide](https://helm.sh/docs/intro/quickstart/)
* [Using Helm Guide](https://helm.sh/docs/intro/using_helm/)
