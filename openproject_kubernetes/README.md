OpenProject :


Deployment of OpenProject using kubernetes

This Project uses official documentation and stable 10 version of OpenProject  from below mentioned OpenProject Repository

PREREQUISITE:

This guide is written assuming that you're starting with:

    • a clean new installation of Ubuntu (64 bit)

    • properly configured DNS that resolves requests to your domain name

    • We are assuming that you have docker and dokcer-compose installed on your system.

    • You have a kubernetes cluster deployed.

STEP FOR INSTALLATION:

First, you must clone the  repository:

	Https://github.com/iVolve-Tech/bts-collaboration.git
Navigate to kubernetes folder

Creating Volumes for Persistent Storage:

if you want to run OpenProject in production you will likely want to ensure that your data is not lost if you restart the container.
To achieve this, we recommend that you create a directory on your host/node system where the Kubernetes is installed (for instance: /var/lib/openproject) where all this data will be stored. We use Persistent Volume and Persistent Volume Claim for above requirement, you can see respective yamls in our kubernetes directory for creating persitent volume.
 
You can use the following commands to create the local directories where the data will be stored across Pod restarts, and start the Pods with those directories mounted:

	sudo mkdir -p /var/lib/openproject/{pgdata,opdata} 

For setting up Ingress
 
   • We use below links to configure Ingress Controller

           https://docs.traefik.io/v1.7/user-guide/kubernetes/

           https://kubernetes.io/docs/concepts/services-networking/ingress/
   
   • Navigate to ingressController folder and execute , we use Cluster Role Biniding and Daemon Set. Use below command to set up Ingress Controller
  
        kubectl apply -f .


Add a TLS Certificate to the Ingress

Create ssl certificates using openssl and replace your .crt and .key file here in secret.yaml

apiVersion: v1
kind: Secret
metadata:
  name: testsecret-tls
  namespace: default
data:
  tls.crt: base64 encoded cert
  tls.key: base64 encoded key
type: kubernetes.io/tls


Now You are done with for the required configuration , now use below command to create our kuberetes resources

	kubectl create -f .



