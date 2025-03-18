## Deploying a Simple Web Application Using NGINX on Kubernetes (Minikube)

### Overview

NGINX (Engine-X) is a high-performance, event-driven web server designed to efficiently handle a large number of concurrent connections. This guide covers:
1. Deploying an NGINX web server on Kubernetes
2. Exposing it via a Service
3. Routing traffic using Ingress
4. Configuring local DNS resolution

### Step 1: Deploy NGINX Using a Deployment

A Deployment ensures that multiple replicas of the NGINX server are managed automatically. The deployment file is located in deployment.yaml

- Apply the Deployment: kubectl apply -f deployment.yaml`
- Verify the Deployment: kubectl get deployments

### Step 2: Expose NGINX Using a Kubernetes Service

To make the NGINX application accessible within the cluster, we create a Service. The service file is located in service.yaml

- Apply the Service: kubectl apply -f service.yaml
- Verify the Service: kubectl get svc
- Visit the Service via ClusterIP: minikube service nginx-service --url  (please note you are using the name of the service and not the name of the yaml file)
The output is like this:
   ######                            http://127.0.0.1:52318 



### Step 3: Enable Ingress in Minikube
To expose our application externally, we use Ingress because of the following reasons:
a) better security by handling SSL termination and access control.
b) Centralized traffic management for multiple services.
c) Cleaner URLs

- Enable Ingress in Minikube: minikube addons enable ingress
- Verify that the Ingress Controller is Running: kubectl get pods -n ingress-nginx (the default namespace for the ingress is ingress-nginx)


Expected output (look for ingress-nginx-controller in Running status):

######        ingress-nginx-controller-56d7c84fd4-rw6kc       1/1         Running         0               84m

### Step 4: Configure Ingress for External Access

Now, let's define an Ingress resource to direct external traffic to our nginx-service. The service file is located in nginx-ingress.yaml

- Apply the Ingress: kubectl apply -f nginx-ingress.yaml
- Verify the Ingress: kubectl get ingress

### Step 5: Configure Local DNS Resolution

To access nginx.local, we need to map it to 127.0.0.1

- sudo nano /etc/hosts
Then add this command line : 127.0.0.1 nginx.local ; ctrl+x and then Y.

### Step 6: Verify That Ingress Is Working

- Run Minikube Tunnel (Required for External Traffic): minikube tunnel
Ensure to keep this terminal open.
- Test Using curl: curl --resolve "nginx.local:80:127.0.0.1" -i http://nginx.local
  
Expected output: 

###### HTTP/1.1 200 OK
###### ...
###### <h1>Welcome to nginx!</h1>

Finally run this command:
### curl 127.0.0.1

#### Hurray you have just deployed a simple Nginx server








