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