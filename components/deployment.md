# Deployment

All Core Components and NLP Services can be deployed into Kubernetes cluster using Helm charts.&#x20;

{% hint style="info" %}
Before you start, download core Project from [https://gitlab.com/promethistai/flowstorm/](https://gitlab.com/promethistai/flowstorm/)
{% endhint %}

## Kubernetes Cluster

Before you start with deployment you have to ensure that your Kubernetes cluster is appropriately preconfigured and contains all requested resources, including

* **Ingress** component to configure processing of incoming web traffic
* **Certificate Manager and Cluster Issuer** for managing issuing and renewal of SSL certificates used by web hosts
* **Secrets** containing required configuration files with sensitive&#x20;

### Ingress

We recommend **ingress-nginx** from Kubernetes

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
kubectl apply -f deploy/nginx-config.yaml # optional: SSL cipher order setup update (stronger first - Alexa requires it)
helm install ingress-nginx ingress-nginx/ingress-nginx --set controller.service.loadBalancerIP=35.198.81.12 --set rbac.create=true --set controller.publishService.enabled=true
```

###  Certificate Manager

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
# helm v2
helm install --name cert-manager --namespace cert-manager --version v1.0.1 --set installCRDs=true jetstack/cert-manager
# helm v3
helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.0.1 --set installCRDs=true
```

### Cluster Issuer

Cluster certificate issuer have to be installed (e.g. Let's Encrypt) to issue certificate for ingress resources.

```bash
kubectl apply -f deploy/clusterissuer-letsencrypt.yaml
```

### Secrets

Before Core Components can be deployed using Helm, there must be two secrets in place

* `flowstorm` containing runtime configuration for Core components
* `google-sa` containing Google Service Account key

#### How to deploy secrets

```bash
kubectl create secret generic google-sa --from-file=google-sa.key
kubectl create secret generic flowstorm --from-file=app.local.properties
```

####  Example of `app.local.properties` file

```
database.url=mongodb+srv://user:password@mongodb-server.com
illusionist.url=https://illusionist.flowstorm.ai
illusionist.apiKey=xxx
aws.secret-key=xxx
aws.access-key=xxx
mscs.key=xxx
mscs.location=xxx
mailgun.domain=mg-domain.com
mailgun.apikey=xxx
mailgun.baseUrl=https://api.eu.mailgun.net/v3/
```

To see all configuration parameters, see [project setup](project-setup/) page.

## Core Services&#x20;

Helm is the package and deployment manager for Kubernetes.&#x20;

### Helm Chart Variables

Core Components have their Helm charts available as a part of project source code at [https://github.com/flowstorm/core/tree/master/deploy](https://github.com/PromethistAI/core/tree/master/deploy).&#x20;

Following variables can be used to parametrise Kubernetes deployment using Helm charts:

| Name                            | Default                                                                                                                                                                                               | Description                                             |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `baseDomain`                    | `flowstorm.ai`                                                                                                                                                                                        | Base domain for ingress host                            |
| `clusterIssuer`                 | `letsencrypt`                                                                                                                                                                                         | Cluster certificate issuer                              |
| `namespace`                     | `default`                                                                                                                                                                                             | Cluster namespace                                       |
| `imagePullSecrets`              | `promethistai-registry`                                                                                                                                                                               | Secret name of type `kubernetes.io/dockerconfigjson`    |
| `imagePullPolicy`               | `IfNotPresent`                                                                                                                                                                                        | Pulling policy                                          |
| `app.image.name`                | <p><code>registry.gitlab.com/promethistai/</code></p><p><code>flowstorm-core/runner/app</code></p><p><code>registry.gitlab.com/promethistai/</code></p><p><code>flowstorm-core/builder/app</code></p> | Docker image                                            |
| `app.image.tag`                 | `latest`                                                                                                                                                                                              | Docker tag                                              |
| `app.mem`                       | `1024` for runner, `512` for builder                                                                                                                                                                  | Memory limit (-`XmX` java parameter value in megabytes) |
| `app.resources.requests.cpu`    | 0.01                                                                                                                                                                                                  | Minimum CPU requested for pod                           |
| `app.resources.requests.memory` | `1024Mi` for runner, `512Mi` for builder                                                                                                                                                              | Minimum memory for pod                                  |

{% hint style="info" %}
#### URL of core service

If you deploy to the default namespace if will be `core.{{ .Values.baseDomain }}` (e.g. `core.promethist.com`). Deployment into any different namespace will result to URL `core.{{ .Values.namespace }}.{{ .Values.baseDomain }}` (e.g. `core.preview.promethist.com`).
{% endhint %}

### How to Install and Upgrade

{% hint style="info" %}
If you want to deploy to different than default namespace, use -`-set namespace=namespace-name` for `helm` command and `-n namespace-name` for `kubectl` command.
{% endhint %}

```bash
helm upgrade --install flowstorm-runner deploy/runner/app
helm upgrade --install flowstorm-builder deploy/builder/app
```

 If you want to check deployed resources

```bash
kubectl get all -l app=flowstorm-runner
kubectl get all -l app=flowstorm-builder
```

## NLP Services

TBD @jan.pichl
