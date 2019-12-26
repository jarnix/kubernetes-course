https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6045274#questions

# Install sous windows

Open the Hyper-V Manager.

Once in the Hyper-V Manager, on the right panel, select the Virtual Switch Manager.

Next we will create a virtual switch for minikube. Select New virtual network switch on the right hand side, select External for the network type, and then press the Create Virtual Switch button.

Name the switch Primary Virtual Switch and click the apply button.

Once you have the switch created we are now ready to start minikube. Run the following command to start the minikube VM with our applied changes.


# Pour démarrer minikube :

minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"

# Choix du contexte

kubectl config get-contexts

# Lancement d'un pod

kubectl run hello-kubernetes --image=k8s.gcr.io/echoserver:1.4 --port=8080

kubectl expose deployment hello-kubernetes --type=NodePort

kubectl get service hello-kubernetes

=> donne le port

# Install aws cli

https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html

Compte aws : +aws

Dans console.aws : création d'un compte avec l'accès "AdministratorAccess" + access key, puis :

aws2 configure

=> rentrer les infos d'accès

# créer un bucket s3

kops-state-jarnix

# créer la zone dns kubernetes.jarnix.com dans zone53

et déléguer dans mon registrar

# création cluster avec kops

kops create cluster --name=kubernetes.jarnix.com --state=s3://kops-state-jarnix --node-count=2 --node-size=t2.micro --dns-zone=kubernetes.jarnix.com --zones=eu-west-1a

kops update cluster --name kubernetes.jarnix.com --state=s3://kops-state-jarnix --yes

kubectl run hello-kubernetes --image=k8s.gcr.io/echoserver:1.4 --port=8080

# pour le supprimer

kops delete cluster kubernetes.jarnix.com --state=s3://kops-state-jarnix --yes