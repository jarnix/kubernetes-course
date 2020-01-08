Déploiement d'une registry docker sécurisée avec authentification via kubernetes sur GKE

# Cluster et namespace

- Dans GKE, créer un cluster avec 1 node parce qu'on est pauvre ou utiliser un existant.
- Se connecter au cluster pour kubectl

```
kubectl create namespace registry
```

## nginx ingress

Source: 
- https://itsmetommy.com/2019/07/27/kubernetes-nginx-ingress-controller-on-gke/


Si nécessaire, ajouter un repo pour helm

```
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

```
helm install stable/nginx-ingress  --generate-name --namespace=registry --set rbac.create=true   --set controller.publishService.enabled=true --set controller.scope.enabled=true --set controller.scope.namespace=registry
```

Attendre :)
```
kubectl --namespace registry get services -o wide nginx-ingress-1578510202-controller
```

```
kubectl get svc --namespace registry
```

On obtient l'ip d'ingress.

Ajouter cette IP aux DNS par exemple ici registry.jarnix.com

# Letsencrypt 
## cert-manager

```
kubectl create namespace cert-manager
```

```
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.12.0/cert-manager.yaml
```

Récupérer l'identité qu'on utilise pour être sûr ;)

```
gcloud config get-value core/account
```

## Vérifier l'install
```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=<votre user>
```

```
kubectl get pods --namespace cert-manager
```

On fait un test avec un certif auto signé

```
kubectl apply -f cert-test.yaml
```

```
kubectl describe certificate -n cert-manager-test
```

Un peu de ménage :
```
kubectl delete -f cert-test.yaml
```

Configurer le issuer

```
kubectl apply -f cert-issuer.yaml
```

Configurer le certificate pour le challenge http

```
kubectl apply -f cert-certificate.yaml
```

Vérifier :

```
kubectl describe certificate registry-jarnix-com
```

Après quelques minutes, le certificat est disponible

# Docker registry

# Auth

# Déploiement


Sources :
- https://www.nearform.com/blog/how-to-run-a-public-docker-registry-in-kubernetes/

To be continued :
- https://medium.com/containerum/how-to-launch-nginx-ingress-and-cert-manager-in-kubernetes-55b182a80c8f
- https://cert-manager.io/docs/configuration/acme/
- https://medium.com/contino-engineering/how-to-automatic-ssl-certificate-management-for-your-kubernetes-application-deployment-94b64dfc9114
- https://www.nearform.com/blog/how-to-run-a-public-docker-registry-in-kubernetes/ 