Helm Charts Installer
=====================
[![](https://img.shields.io/pypi/v/helm-charts.svg)](https://pypi.org/project/helm-charts/)
[![](https://img.shields.io/pypi/l/helm-charts.svg?colorB=blue)](https://pypi.org/project/helm-charts/)
[![](https://img.shields.io/pypi/pyversions/helm-charts.svg)](https://pypi.org/project/helm-charts/)

Install various Kubernetes Helm charts on a Kubernetes cluster,
This application is mainly intended for local cluster charts installations.     
**Important** helm_charts supports python3.5+ only!

- [Traefik](https://traefik.io/) (As Ingress Controller)
- [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
- [OpenFaaS](https://www.openfaas.com/)
- [Airflow](https://airflow.apache.org/)
- More to come

Prerequisites
-------------

Install Docker **Edge** version,
Follow instructions [here](https://store.docker.com/editions/community/docker-ce-desktop-mac) (MacOS), 
Enter Docker preferences, And make sure to activate Kubernetes.
![](docs/docker_kubernetes.png)

* The application assumes that file `~/.kube/config` created/appended is generated by Docker installation,
  And the config file contains context of `docker-desktop`

Installation
------------


Run
```bash
pip3 install helm-charts
```

Usage
-----
**Execute** application   
`helm_charts [-h] [--config-file CONFIG_FILE] [--use-context USE_CONTEXT] [--helm-init]`

Examples
--------

**Specify** config file, default file is: `~/.kube/config`  
`helm_charts --config-file PATH_TO_CONFIG_FILE`

**set** cluster context, default context is: `docker-desktop`  
`helm_charts --use-context CONTEXT_NAME`

**set** custom charts installation file:  
`helm_charts --supported-charts-file PATH_TO_FILE`  
for detail see below

also **Executes** 'helm init' command  
`helm_charts --helm-init`

Custom Charts File
------------------
if using the `--supported-charts-file` flag,  
file structure **must** be as:
```yaml

- chart_name: ingress-traefik
  helm_repo_name: stable/traefik
  name_space: ingress-traefik
  values_file: ingress-traefik.values.local.yml
  private_image: False
  # extra_executes:
  #   - kubectl add something
  #   - some command with flags
  extra_executes: []

- chart_name: chart_name_to_install
  helm_repo_name: my_private_repo/some-chart
  name_space: kube-system
  values_file: kubernetes-dashboard.values.local.yml
  private_image: False
  # extra_executes:
  #   - kubectl add something
  #   - some command with flags
  extra_executes: []

...
```
`values_file` file **must** be present in the same directory,  
all keys **must** be strings, `private_image` **must** be boolean 
, and `extra_executes` **must** be list

Access Kubernetes Dashboard
---------------------------

If `kubernetes_dashboard` selected during installation process,
In order to login, access https://kubernetes.localhost
Press `Choose kubeconfig file` or `...` on right side,  
Select `~/.kube/config` file and press `SIGN IN`.  
Also possible to choose `SKIP` as local installation allow this
![](docs/kubernetes_dashboard.png)
