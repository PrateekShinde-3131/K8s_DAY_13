# Static pods, manual scheduling, labels, and selectors in Kubernetes 📘🚀
## 1. Create a Pod Manually without a Scheduler
You can manually create a pod on the control plane by placing a manifest in the static pod manifest directory (/etc/kubernetes/manifests). Here's how to do it:
- SSH into the control plane node:
```
ssh user@control-plane-node
```
- Go to the static pod manifest directory:
```
cd /etc/kubernetes/manifests/
```
- Create a manifest file for the pod (static-pod.yaml):
```
sudo nano static-pod.yaml
```
- Add the following YAML configuration:
```
apiVersion: v1
kind: Pod
metadata:
  name: static-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```
- Save and exit the file. The Kubernetes control plane will detect the manifest and create the pod automatically.

## 2. Restart the Control Plane Components
To restart the control plane components (like kube-apiserver, kube-scheduler, etc.):
- Use systemctl to restart the services on the control plane node:
```
sudo systemctl restart kubelet
sudo systemctl restart kube-apiserver
sudo systemctl restart kube-scheduler
sudo systemctl restart kube-controller-manager
```
## 3. Create Pods with Specific Labels
Now, create three pods (pod1, pod2, and pod3) based on the nginx image, each with a different environment label (env: test, env: dev, env: prod):
- Create the following manifest (pods.yaml):
```
apiVersion: v1
kind: Pod
metadata:
  name: pod1
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: pod2
  labels:
    env: dev
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: pod3
  labels:
    env: prod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```
- Apply the manifest:
```
kubectl apply -f pods.yaml
```
## 4. Filter Pods with Specific Labels (env: dev and env: prod)
To filter the pods based on their labels, use kubectl commands:
- Get pods with the label env=dev:
```
kubectl get pods -l env=dev
```
- Get pods with the label env=prod:
```
kubectl get pods -l env=prod
```
