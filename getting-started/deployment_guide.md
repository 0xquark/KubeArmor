# Deployment Guide

## Deployment Steps
### 1. Download and install karmor cli-tool
```
curl -sfL https://raw.githubusercontent.com/kubearmor/kubearmor-client/main/install.sh | sudo sh -s -- -b /usr/local/bin
```

### 2. Install KubeArmor
```
karmor install
```
<details>
  <summary>Output of karmor install</summary>

```
aws@pandora:~$ karmor install
Auto Detected Environment : docker
CRD kubearmorpolicies.security.kubearmor.com ...
CRD kubearmorhostpolicies.security.kubearmor.com ...
Service Account ...
Cluster Role Bindings ...
KubeArmor Relay Service ...
KubeArmor Relay Deployment ...
KubeArmor DaemonSet ...
KubeArmor Policy Manager Service ...
KubeArmor Policy Manager Deployment ...
KubeArmor Host Policy Manager Service ...
KubeArmor Host Policy Manager Deployment ...
```
</details>

It is assumed that the k8s cluster is already present/reachable setup with the [*required prerequisites*](#Prerequisites) and the user has rights to create service-accounts and cluster-role-bindings.

### 3. Deploying sample app and policies
   
#### a. Deploy sample [multiubuntu app](https://github.com/kubearmor/KubeArmor/blob/master/examples/multiubuntu.md)
```
kubectl apply -f https://raw.githubusercontent.com/kubearmor/KubeArmor/master/examples/multiubuntu/multiubuntu-deployment.yaml
```
#### b. Deploy [sample policies](https://github.com/kubearmor/KubeArmor/blob/master/getting-started/security_policy_examples.md)
```
kubectl apply -f https://raw.githubusercontent.com/kubearmor/KubeArmor/master/examples/multiubuntu/security-policies/ksp-group-1-proc-path-block.yaml
```
This sample policy blocks execution of `sleep` command in ubuntu-1 pods.
#### c. Simulate policy violation
```
$ kubectl -n multiubuntu exec -it POD_NAME_FOR_UBUNTU_1 -- bash
# sleep 1
(Permission Denied)
```
Substitute POD_NAME_FOR_UBUNTU_1 with the actual pod name from `kubectl get pods -n multiubuntu`.

### 4. Getting Alerts/Telemetry from KubeArmor
#### a. Enable port-forwarding for KubeArmor relay
```
kubectl port-forward -n kube-system svc/kubearmor 32767:32767
```
#### b. Observing logs using karmor cli
```
karmor log
```

## Manual YAML based [KubeArmor deployment](https://github.com/kubearmor/KubeArmor/tree/main/deployments)
1. [EKS](https://github.com/kubearmor/KubeArmor/tree/main/deployments/EKS)
2. [GKE](https://github.com/kubearmor/KubeArmor/tree/main/deployments/GKE)
3. [docker](https://github.com/kubearmor/KubeArmor/tree/main/deployments/docker)
4. [generic](https://github.com/kubearmor/KubeArmor/tree/main/deployments/generic)
5. [k3s](https://github.com/kubearmor/KubeArmor/tree/main/deployments/k3s)
6. [microk8s](https://github.com/kubearmor/KubeArmor/tree/main/deployments/microk8s)
7. [minikube](https://github.com/kubearmor/KubeArmor/tree/main/deployments/minikube)

## K8s platforms tested
1. Google Kubernetes Engine (GKE) Container Optimized OS (COS)
2. GKE Ubuntu image
3. [Amazon Elastic Kubernetes Service (EKS)](https://github.com/kubearmor/KubeArmor/tree/master/deployments/EKS)
4. Self-managed (on-prem) k8s
5. Local k8s engines (microk8s, k3s, minikube)

## Prerequisites
1. [Amazon Elastic Kubernetes Service (EKS)](https://github.com/kubearmor/KubeArmor/tree/master/deployments/EKS#prerequisite-for-the-deployment)
2. [Minikube](https://github.com/kubearmor/KubeArmor/tree/master/contribution/minikube#minikube-installation)
