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
- Add gen3 helm repo:
```
helm repo add gen3 https://helm.gen3.org
helm repo update
```
- Create namespace:
```
kubectl create namespace gen3-ns
```
- Deploy our custom `minimal_values.yaml` helm chart:
```
helm uninstall gen3 -n gen3-ns
helm upgrade --install gen3 gen3/gen3 -f minimal_values.yaml -n gen3-ns
```
- Check pods (all should be running at some point - wait around 2 minutes):
```
kubectl get pods -n gen3-ns | grep deployment
```

- Add mapping to /etc/hosts:
```
echo "127.0.0.1 mygen3dev.se4.bio" | sudo tee -a /etc/hosts
```
- Map internal kube cluster port 80 of internal revproxy pod to 8080 localhost:
```
kubectl get svc -n gen3-ns
kubectl port-forward svc/revproxy-service 8080:80 -n gen3-ns
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


# Extra: integrating Fence with IDP

This simple setup can be taken a step further, by letting Fence
integrate with the local IDP.

1. Add a "fence-config" secret based on your local Fence config yaml:
```
kubectl create secret generic fence-config \
  --from-file=/path/to/your/fence-config.yaml
```

See also https://github.com/uc-cdis/fence/blob/master/fence/config-default.yaml for the configuration that Fence supports.
