# Kubernetes course

Open the Hyper-V Manager.

Once in the Hyper-V Manager, on the right panel, select the Virtual Switch Manager.

Next we will create a virtual switch for minikube. Select New virtual network switch on the right hand side, select External for the network type, and then press the Create Virtual Switch button.

Name the switch Primary Virtual Switch and click the apply button.

Once you have the switch created we are now ready to start minikube. Run the following command to start the minikube VM with our applied changes.


Pour démarrer minikube :

minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"
