
# Kind8s Guidelines

This is my own cheat sheet to build and manage my own local kubernetes clusters using Kind (Kubernete in Docker) 


## 1. Install Kind

- First of all we need to install kind in our machine using this command :
```bash
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.25.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

- Verify kind has been installed successfully by running this command :
```bash
kind version
```
***This command will show you the current version of the installed kind***

## 2. Create Kind K8s Cluster
After kind installed on your machine, now let's create our first cluster in kind using my `./kind-cluster-config.yaml` in this repository by running this command :
```bash
kind create cluster --config kind-cluster-config.yml
```
***This command will give you a cluster with one control plane & 2 worker node***

To verify your cluster created successfully you can run this command *(This command will show you a list of created clusters)* :
```bash
kind get clusters
```

To get list of nodes in your created cluster you can run this command :
```bash
kind get nodes
```
All set, now you have a k8s cluster created on your local machine by using Kind

## 3. Install Kubectl & Helm
To get native kubernetes experience we should install `kubectl` and `helm` to manage our k8s clusters.

- Install Kubectl
```bash
sudo snap install kubectl --classic
```
Verify kubectl was successfully isntalled by running this command 
```bash
kubectl version
```

- Install Helm
```bash
sudo snap install helm --classic
```
Verify helm was successfully isntalled by running this command 
```bash
helm version
```

## 4. Install Portainer as GUI Manager
To easily manage and monitor our cluster we can install portainer using helm charts.
```bash
helm repo add portainer https://portainer.github.io/k8s/
```
After this update the helm repository using this command :
```bash
helm repo update
```
Once the update completes, you're ready to begin the installation. Install portainer using this command :
```bash
helm repo update
```

To check if you Portainer services are running, run this this command :
```bash
helm get all -n portainer
```

Check the created namespace with this command : 
```bash
helm list --all-namespaces
```

Now you have portainer installed using helm, the last step is you shoul expose your portainer port to can access it from your web browser using this command :
```bash
kubectl port-forward -n portainer svc/portainer 9010:9000 &
```

Now open your web browser and visit this link [http:0.0.0.0:9010](http:0.0.0.0:9010), you gonna see your portainer dashboard showed !

To kill portainer process, use this command :
```bash
ps aux | grep "kubectl port-forward"
```
It wil list running process and with process id listed

Kill the process id using this command :
```bash
kill <pid>
```

## 5. Renew Certificate Error

If you cannot run kubectl correctly because of certificate expired, you can fix it by running this command on your kind control-plane container
```bash
kubeadm certs renew all
```

To check certificate expiration you can use this command : 
```bash
kubeadm certs check-expiration
```