# registry avec tls dans kubernetes

Se connecter au cluster pour kubectl (remplacer le nom du cluster)

```
gcloud container clusters get-credentials testju-cluster --zone europe-west1-b --project webediads
```

Créer deux adresses ip

```
gcloud compute addresses create registryipaddress --global
gcloud compute addresses create authipaddress --global
```

L'obtenir :
```
gcloud compute addresses describe registryipaddress --global
gcloud compute addresses describe authipaddress --global
```

#### Certificat pour l'auth
On l'utilisera dans une entrée A pour authregistry.monndd.com

Pour générer le certificat auto-signé pour la communication entre registry et auth :

```
cd ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.pem
```

=> Utiliser le NDD final pour le Common Name

#### Création des secret

<!-- base64 des deux fichiers générés => deux longues chaînes dans auth-ssl-server-secret.yaml*

```
base64 -w 0 server.key
base64 -w 0 server.pem
```

```
kubectl apply -f auth-ssl-server-secret.yaml
``` -->

```
cd ssl
kubectl create secret generic auth-ssl-server --from-file=./server.key --from-file=./server.pem
```

#### Créer le volume pour la registry

```
kubectl apply -f registry-data-persistent-volume.yaml
```

#### Créer le secret pour l'acl

Ajouter l'entrée pour les users dans la config config/auth_config.yml

```
kubectl create secret generic auth-config-secret --from-file=./config/auth_config.yml
```


<!-- L'encoder en base64

```
base64 -w 0 config/auth_config.yml
```

Modifier le yaml auth-config-secret.yaml

```
kubectl apply -f auth-config-secret.yaml
``` -->

#### Pod registry + auth

```
kubectl apply -f registry-pod.yaml
kubectl apply -f auth-pod.yaml
```

#### Certificat managé par GKE

Mettre le bon nom de domaine dans le fichier registry-cert-managed.yaml

```
kubectl apply -f registry-cert-managed.yaml
```

<!-- Mettre le bon nom de domaine dans le fichier auth-cert-managed.yaml

```
kubectl apply -f auth-cert-managed.yaml
``` -->

```
kubectl describe managedcertificate
```

#### Créer le service et l'ingress avec le certificat managé

```
kubectl apply -f registry-service.yaml
kubectl apply -f auth-service.yaml
```

```
kubectl apply -f registry-managed-ingress.yaml
```

<!-- ```
kubectl apply -f auth-managed-ingress.yaml
``` -->

```
kubectl get ingress
```

Récupérer l'ip du service de type load balancer créé pour auth :

```
kubectl get svc
```
