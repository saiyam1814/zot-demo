# zot-demo
## Create Kubernetes cluster (In demo we have used Civo Kubernetes)

## Install cert manager 
`kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.0/cert-manager.yaml`

## Install Nginx nginx controller 
```
version=$(curl -s "https://api.github.com/repos/kubernetes/ingress-nginx/releases" | grep tag_name | grep controller | sort -r | awk -F'"' 'NR==1 {print $4}')

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/${version}/deploy/static/provider/cloud/deploy.yaml
```

## Install Zot 

```
helm repo add project-zot http://zotregistry.io/helm-charts
helm repo update project-zot
helm install zot project-zot/zot -f values.yaml
```

## Push and Pull 

```
First install Skopeo (brew install skopeo)
skopeo --insecure-policy copy --dest-tls-verify=false --multi-arch=all \
   --format=oci docker://saiyam911/wsmblr --dest-creds admin:admin \
   docker://zot.<change to your nginx LB DNS>/saiyam:latest
```
