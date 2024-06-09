# Overview
An Ansible playbook that installs Kubernetes

# Features
- containerd
- calico for pod networking

# Quickstart
```
ssh-keyscan -H -t rsa c1-cp1.lab.local >> ~/.ssh/known_hosts
ssh-keyscan -H -t rsa c1-node1.lab.local >> ~/.ssh/known_hosts
ssh-keyscan -H -t rsa c1-node2.lab.local >> ~/.ssh/known_hosts
ssh-keyscan -H -t rsa c1-node3.lab.local >> ~/.ssh/known_hosts

ansible -i inventory/dev all -m ping --user appuser

ansible-playbook -i inventory/dev playbooks/k8s_all.yaml 

# reboot might be required after installation
ansible -i inventory/dev all -a "/sbin/reboot" --become

# Check the nodes status
kubectl cluster-info
kubectl get nodes

# Verify the status of pods in kube-system namespace
kubectl get pods -n kube-system

# OPTIONAL: 

# Installing Pre-reqs (+)
ansible-playbook -i inventory/dev playbooks/pre-reqs.yaml --user appuser

# Installing Longhorn (+)
ansible-galaxy collection install community.kubernetes
ansible-playbook -i inventory/dev playbooks/longhorn.yaml --user appuser

# Installing Istio https://www.tigera.io/blog/how-to-build-a-service-mesh-with-istio-and-calico/ (-)
ansible-playbook -i inventory/dev playbooks/install_istio.yaml --user appuser

# Setup BPG to Firewall (*)
ansible-playbook -i inventory/dev playbooks/configure_bgp.yaml --user appuser 

# Setup Metrics-Server (*)
ansible-playbook -i inventory/dev playbooks/metrics-server.yaml --user appuser

notes: 
+ test ok
- not tested
* dont work

```