**Kubernetes**

- A cluster is a set of nodes which are either physical or virtual servers (like a Docker swarm). At
  least one master (like a swarm manager) controls all nodes through the Kubernetes API server.
  Communication with the master is handled by the kubectl CLI tool.
- Each node runs an agent process known as a kubelet. It is responsible for receiving information
  from the master to start, stop, or modify groups of containers known as pods which have shared
  storage and network resources.

- Your host may have their own Kubernetes methods and recommendations, so it will be practical
  to provision and configure a test platform within the same environment.
- Another option is the play with Kubernetes tool allows you configure a cluster without installing
  anything locally.
- Finally, it’s possible to install Kubernetes on your Windows, macOS, or Linux development PC.
  Kubernetes is provided in Docker Desktop for Windows and macOS: to enable it, choose
  Settings from the Docker icon menu, select the Kubernetes pane, check Enable Kubernetes,
  and click Apply & Restart. This process may take several minutes to complete.

**Play with Kubernetes Link** - https://labs.play-with-k8s.com/

**Kubernetes deployment**

- Kubernetes deployments are defined YAML files. Unlike traditional programming steps, these
  describe a desired end state which Kubernetes aims to achieve.
- The following example defines a deployment.yaml file to run the latest NGINX web server in
  two replica pods:

\*\*
apiVersion: apps/v1
kind: Deployment
metadata:
name: nginx-deployment
spec:
selector:
matchLabels:
app: nginx
replicas: 2 # 2 pods
template:
metadata:
labels:
app: nginx
spec:
containers:

- name: nginx
  image: nginx:latest
  ports:
- containerPort: 80
- \*\*

To deploy:

- kubectl apply -f ./deployment.yaml

You can also host the deployment file on a web server and use its URL as the reference, e.g.

- kubectl apply -f https://myserver.com/deployment.yaml

Information about the nginx-deployement can be displayed with:

- kubectl describe deployment nginx-deployment

The pods created during the deployment can be listed:

- kubectl get pods -l app=nginx

to reveal pod names:
NAME READY STATUS RESTARTS AGE
nginx-deployment-1771418926-7o5ns
1/1
Running
0
10h
nginx-deployment-1771418926-r18az
1/1
Running
0
10h

More information about any pod can then be viewed, e.g.

- kubectl describe pod nginx-deployment-1771418926-7o5ns

Node IP addresses and ports are only available within the cluster. A load balancer can be added
to expose them externally:

- kubectl expose deployment nginx-deployment --port=80 --target-port=80 --name=nginxlb --type=LoadBalancer

**Deployment updates**
To update the deployment, change the YAML file and run the same kubectl apply again. This
can be useful when you need to increase the number of replicas owing to increased demand.

Deployments can be stopped and deleted by passing its name to the delete command:

- kubectl delete deployment nginx-deployment

Kubernetes resources:

1. Kompose: a tool to convert Docker Compose to Kubernetes configuration files
2. minikube: quickly define a Kubernetes cluster on a development machine
3. Helm: manages Kubernetes resources
