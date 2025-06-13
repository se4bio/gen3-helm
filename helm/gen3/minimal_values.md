# Minimal setup steps

## Pre-requisites

- Install docker, minikube, helm
- Start docker
- Start minikube: `minikube start`

Clone our gen3-helm fork and checkout `minimal_setup` branch:

```
git clone git@github.com:se4bio/gen3-helm.git
git fetch --all
git checkout minimal_setup
```

## Installation

- Go to folder `./gen3-helm/helm/gen3/`
- Deploy our custom `minimal_values.yaml` helm chart:
```
helm upgrade --install gen3 gen3/gen3 -f minimal_values.yaml
```
- Check pods (all should be running at some point):
```
kubectl get pods | grep deployment
```
- Wait ~40min for the portal to build. Progress can be followed with:
```
kubectl logs -f deploy/portal-deployment
```

- Add mapping to /etc/hosts:
```
echo "127.0.0.1 mygen3dev.se4.bio" | sudo tee -a /etc/hosts
```
- Map internal kube cluster port 80 of internal revproxy pod to 8080 localhost:
```
kubectl port-forward svc/revproxy-service 8080:80
```
- Test:
```
curl mygen3dev.se4.bio:8080
```
and/or navigate to http://mygen3dev.se4.bio:8080 

# References

- https://docs.gen3.org/gen3-resources/operator-guide/helm/#getting-started-with-helm-quick-start-videos

# Useful commands

- Uninstall: `helm uninstall gen3`
- Check status: `helm status gen3`
- View pods and ports: `kubectl get svc`
