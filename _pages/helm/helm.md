---
title: "mox helm repository"
excerpt: "mox helm repository homepage"
permalink: /helm/
date: 2020-04-17T23:54:20+02:00
last_modified_at: 2020-04-28T00:21:27+02:00
toc: true
categories:
  - devops
tags:
  - helm
  - kubernetes
---

[![](https://github.com/javimox/helm-charts/workflows/Release%20Charts/badge.svg?branch=master)](https://github.com/javimox/helm-charts/actions)

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
 * [hub.helm.sh](https://hub.helm.sh/charts/mox/jira-software)
 * [artifacthub.io](https://artifacthub.io/package/chart/mox/jira-software)
 
If you find any errors, feel free to open an issue on [my GitHub](https://github.com/javimox/helm-charts/tree/master).

## Chart Sources

* [charts/confluence-server](/helm/charts/confluence-server): Atlassian Confluence Server chart
* [charts/jira-software](/helm/charts/jira-software): Atlassian Jira Software chart

## How mox repo works

This repository uses Github actions. Before a chart can be added, it is linted and tested in a kind Kubernetes Cluster.  Following checks will run:

* Chart-testing (lint)
* Create kind cluster
* Chart-testing (install)

When all the check have passed, the PR will be confirmed and the index of this helm repository will be updated with its new content.

### Actions

* [@helm/kind-action](https://github.com/helm/kind-action)
* [@helm/chart-testing-action](https://github.com/helm/chart-testing-action)
* [@helm/chart-releaser-action](https://github.com/helm/chart-releaser-action)

See [Results](#results) for more details.

## Collaborate

If you want to collaborate by adding your Helm charts to this repository, follow these steps:

* Clone me: `git@github.com:javimox/helm-charts.git`
* Create a branch off master named after the chart
* Commit and push origin the new branch
* Create a Pull Request

### <a name="results"></a>Results

* The [Lint and Test Charts](https://github.com/javimox/helm-charts/blob/master/.github/workflows/lint-test.yaml) workflow uses [@helm/kind-action](https://www.github.com/helm/kind-action) GitHub Action to spin up a [kind](https://kind.sigs.k8s.io/) Kubernetes cluster, and [@helm/chart-testing-action](https://www.github.com/helm/chart-testing-action) to lint and test the charts on every Pull Request and push
  
* The [Release Charts](https://github.com/javimox/helm-charts/blob/master/.github/workflows/release.yaml) workflow uses [@helm/chart-releaser-action](https://www.github.com/helm/chart-releaser-action) to turn this GitHub project into a self-hosted Helm chart repo. It does this – during every push to `master` – by checking each chart in this project, and whenever there's a new chart version, creates a corresponding [GitHub release](https://help.github.com/en/github/administering-a-repository/about-releases) named for the chart version, adds Helm chart artifacts to the release, and creates or updates an `index.yaml` file with metadata about those releases, which is then hosted on GitHub Pages

## Credits

* [Lint and Test Charts](https://github.com/helm/charts-repo-actions-demo.git)  

## TODO

Add mox repository to [hub.helm.sh](https://hub.helm.sh)

## License

Copyright (c) 2020 mox.sh

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
