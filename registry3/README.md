# registry avec tls dans kubernetes

Se connecter au cluster pour kubectl (remplacer le nom du cluster)

```
gcloud container clusters get-credentials testju-cluster --zone europe-west1-b --project webediads
```

Créer une adresse ip

```
gcloud compute addresses create registry-ip-address --global
```

Pour générer le certificat auto-signé pour la communication entre registry et auth, ou utiliser ceux déjà dans le dossier ssl

```
cd ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.pem
```

=> Utiliser "auth" pour le Common Name

#### Création des secret

base64 des deux fichiers générés => deux longues chaînes dans auth-ssl-server-secret.yaml*

```
base64 -w 0 server.key
base64 -w 0 server.pem
```

```
kubectl apply -f auth-ssl-server-secret.yaml
```

Exemple d'utilisation dans un pod :

```
kubectl apply -f access-pod.yaml
```


== Config auth




== Créer les volumes

kubectl apply -f *-volume.yaml

== Certificat
kubectl describe managedcertificate

kubectl get ingress


Sources :

https://cloud.google.com/kubernetes-engine/docs/how-to/managed-certs