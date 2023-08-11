![Gitlab](https://github.com/mitspap123/ml-gitlab-ci-pipelines-exporter/blob/main/docs/gitlab-logo-no-bkgrd-resized.png)

## The DevSecOps Platform Exporter

**ml-gitlab-ci-pipelines-exporter** project allows you to monitor your GitLab CI pipelines scraping metrics with Prometheus and build Grafana Dshboards

You can find more information on [GitLab docs](https://docs.gitlab.com/ee/ci/pipelines/pipeline_efficiency.html#pipeline-monitoring) about improving your pipeline efficiency.

Here are some custom Grafana dashboards created for using those metrics. You can sort or filter by project, by ref (branch name) or by owner of the project.

![Image1](https://github.com/mitspap123/ml-gitlab-ci-pipelines-exporter/blob/main/docs/running-failed-not-completed-pipelines.png)
<br>
![Image2](https://github.com/mitspap123/ml-gitlab-ci-pipelines-exporter/blob/main/docs/successfully-completed-pipelines.png)
<br>
## Install

### Kubernetes

If you want to install and run it on kubernetes, there is a [helm chart](https://artifacthub.io/packages/helm/mvisonneau/gitlab-ci-pipelines-exporter) available for this purpose.

In the .k8s folder of the project, there are three folders `dev|stage|prod` for every environment. In the manifests of every folder there are two instances for back-end services and front-end deployments.

**Configuration**

- Config-map <br />
The `main` | `master` branch are monitored for Dev and Stage environment and `production` branch, `tags` are excluded from scraping and `MRs` as well.

- Config Secret <br />
GitLab Token to use to authenticate against the API, it requires api and read_repository permissions for that,

- Deployment <br />
Release Version v0.5.5

- Service Monitor <br />
Target endpoint is http-services port (8080) and scraping prometheus metrics every 10 seconds.

## Git Project commands

```
cd existing_repo
git remote add origin https://github.com/mitspap123/ml-gitlab-ci-pipelines-exporter.git
git branch -M main
git push -uf origin main
```

## Test and Deploy

Redis replicaset is not necessary for the exporter, the configuration is optional and solely useful for an HA setup. By default the data is held in the memory of the exporter.
<br>
<br>
After deploying the exporter you should be able to see the following logs

```
INFO[0000] starting exporter                             gitlab-endpoint="https://gitlab.com" on-init-fetch-refs-from-pipelines=true pulling-pipelines-every=60s pulling-projects-every=15s pulling-refs-every=10s pulling-workers=2 rate-limit=10rps
INFO[0000] configured wildcards                          count=1
INFO[0000] found new project                             project-name=foo/project wildcard-archived=false wildcard-owner-include-subgroups=false wildcard-owner-kind=group wildcard-owner-name=foo wildcard-search=
INFO[0000] found new project                             project-name=foo/bar wildcard-archived=false wildcard-owner-include-subgroups=false wildcard-owner-kind=group wildcard-owner-name=foo wildcard-search=
INFO[0000] configured projects                           count=3
INFO[0000] started, now serving requests                 listen-address=":8080"
INFO[0000] found project refs                            project-path-with-namespace=foo/project project-ref=main
INFO[0000] found project refs                            project-path-with-namespace=bar/project project-ref=main
INFO[0000] found project refs                            project-path-with-namespace=foo/bar project-ref=main
```