# Istio Helm charts

This turns the Istio Helm charts bundled in the Istio repository into hosted Helm charts.

## Installation

To start using the Helm charts from this repository run the following command

```bash
helm repo add estafette https://helm.estafette.io
```

To update and view all available charts in the repository run

```bash
helm repo update estafette
helm search repo estafette
```

From here on you can install or upgrade helm charts as follows

```bash
kubectl create namespace istio-system
helm upgrade --install istio-base estafette/istio-base --namespace istio-system --wait
helm upgrade --install istio-discovery estafette/istio-discovery --namespace istio-system --wait
helm upgrade --install istio-ingress estafette/istio-ingress --namespace istio-system --wait
helm upgrade --install istio-egress estafette/istio-egress --namespace istio-system --wait
```

More details on installing and upgrading Istio using Helm can be found at:

* https://istio.io/latest/docs/setup/install/helm/
* https://istio.io/latest/docs/setup/upgrade/helm/


### CRDs

With Helm 3 CRDs present in a directory _crds_ are automatically installed. See https://helm.sh/docs/topics/chart_best_practices/custom_resource_definitions/.

When managing helm charts with [helmfile](https://github.com/roboll/helmfile) validation of `istio-discovery` can fail due to the CRDs in `istio-base` not being installed yet. To avoid this add `disableValidation: true` to the releases depending on those CRDs. The helmfile manifest would look something like this:

```yaml
helmDefaults:
  wait: true
  atomic: true
  timeout: 120

repositories:
- name: estafette
  url: https://helm.estafette.io

releases:
- name: istio-base
  namespace: istio-system
  chart: estafette/istio-base
  version: 1.11.1
- name: istio-discovery
  namespace: istio-system
  chart: estafette/istio-discovery
  version: 1.11.1
  disableValidation: true
  needs:
  - istio-system/istio-base
- name: istio-ingress
  namespace: istio-system
  chart: estafette/istio-ingress
  version: 1.11.1
  disableValidation: true
  needs:
  - istio-system/istio-system
- name: istio-egress
  namespace: istio-system
  chart: estafette/istio-egress
  version: 1.11.1
  disableValidation: true
  needs:
  - istio-system/istio-system
```