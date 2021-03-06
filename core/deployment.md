# Deployment

All Core Components and NLP Services can be deployed into Kubernetes cluster using Helm charts. 

{% hint style="info" %}
Before you start, download core Project from [https://github.com/flowstorm/core](https://github.com/flowstorm/core)
{% endhint %}

## Kubernetes Cluster

Before you start with deployment you have to ensure that your Kubernetes cluster is appropriately preconfigured and contains all requested resources, including

* **Ingress** component to configure processing of incoming web traffic
* **Certificate Manager and Cluster Issuer** for managing issuing and renewal of SSL certificates used by web hosts
* **Secrets** containing required configuration files with sensitive 

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

Cluster certificate issuer have to be installed \(e.g. Let's Encrypt\) to issue certificate for ingress resources.

```bash
kubectl apply -f deploy/clusterissuer-letsencrypt.yaml
```

### Secrets

Before Core Components can be deployed using Helm, there must be two secrets in place

* `app-local` containing runtime configuration for Core components
* `google-sa` containing Google Service Account key

#### How to deploy secrets

```bash
kubectl create secret generic google-sa --from-file=google-sa.key
kubectl create secret generic app-local --from-file=app.local.properties
```

####  Example of `app.local.properties` file

```text
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

## Core Services 

Helm is the package and deployment manager for Kubernetes. 

### Helm Chart Variables

Core Components have their Helm charts available as a part of project source code at [https://github.com/flowstorm/core/tree/master/deploy](https://github.com/PromethistAI/core/tree/master/deploy). 

Following variables can be used to parametrise Kubernetes deployment using Helm charts:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Default</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>baseDomain</code>
      </td>
      <td style="text-align:left"><code>flowstorm.ai</code>
      </td>
      <td style="text-align:left">Base domain for ingress host</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>clusterIssuer</code>
      </td>
      <td style="text-align:left"><code>letsencrypt</code>
      </td>
      <td style="text-align:left">Cluster certificate issuer</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>namespace</code>
      </td>
      <td style="text-align:left"><code>default</code>
      </td>
      <td style="text-align:left">Cluster namespace</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>imagePullSecrets</code>
      </td>
      <td style="text-align:left"><code>promethistai-registry</code>
      </td>
      <td style="text-align:left">Secret name of type <code>kubernetes.io/dockerconfigjson</code> 
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>imagePullPolicy</code>
      </td>
      <td style="text-align:left"><code>IfNotPresent</code>
      </td>
      <td style="text-align:left">Pulling policy</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>app.image.name</code>
      </td>
      <td style="text-align:left">
        <p><code>registry.gitlab.com/promethistai/</code>
        </p>
        <p><code>flowstorm-core/runner/app</code>
        </p>
        <p><code>registry.gitlab.com/promethistai/</code>
        </p>
        <p><code>flowstorm-core/builder/app</code>
        </p>
      </td>
      <td style="text-align:left">Docker image</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>app.image.tag</code>
      </td>
      <td style="text-align:left"><code>latest</code>
      </td>
      <td style="text-align:left">Docker tag</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>app.mem</code>
      </td>
      <td style="text-align:left"><code>1024</code> for runner, <code>512</code> for builder</td>
      <td style="text-align:left">Memory limit (-<code>XmX</code> java parameter value in megabytes)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>app.resources.requests.cpu</code>
      </td>
      <td style="text-align:left">0.01</td>
      <td style="text-align:left">Minimum CPU requested for pod</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>app.resources.requests.memory</code>
      </td>
      <td style="text-align:left"><code>1024Mi</code> for runner, <code>512Mi</code> for builder</td>
      <td style="text-align:left">Minimum memory for pod</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
#### URL of core service

If you deploy to the default namespace if will be `core.{{ .Values.baseDomain }}` \(e.g. `core.promethist.com`\). Deployment into any different namespace will result to URL `core.{{ .Values.namespace }}.{{ .Values.baseDomain }}` \(e.g. `core.preview.promethist.com`\).
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

