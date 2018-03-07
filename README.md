# Ansible Role: Kubernetes Cluster
=========

This role deploys a multi-master Kubernetes cluster based on 6 nodes, 3 master nodes and 3 worker nodes. It follows the guidelines as described by Kelsey Hightower in his "Kubernetes the hard way" series found here: https://github.com/kelseyhightower/kubernetes-the-hard-way

**This role assumes local servers, and is written for non-cloud environments**

This playbook is still a work in progress but as long as you stick to the 6 nodes it will work.

The Ansible inventory file that I worked with while testing this playbook looks like this:

```
[kubernetes]
<short name of master node 1> ansible_ssh_host=<ip address of master node 1>
<short name of master node 2>  ansible_ssh_host=<ip address of master node 2>
<short name of master node 3>  ansible_ssh_host=<ip address of master node 3>
<short name of worker node 1>  ansible_ssh_host=<ip address of worker node 1>
<short name of worker node 2> ansible_ssh_host=<ip address of worker node 2>
<short name of worker node 3> ansible_ssh_host=<ip address of worker node 3>

[kube-masters]
<ip address of master node 1>
<ip address of master node 2>
<ip address of master node 3>
<short name of master node 1>
<short name of master node 2>
<short name of master node 3>

[kube-workers]
<ip address of worker node 1>
<ip address of worker node 2>
<ip address of worker node 3>
<short name of worker node 1>
<short name of worker node 2>
<short name of worker node 3>
```

## Requirements
------------
This role assumes local servers, and is written for non-cloud environments.

## Role Variables
--------------
```yaml
external_ip: 1.1.1.1 # the external IP address over which the services will be accessible
k8sclustername: ansible-kubernetes # the name of the cluster
kubeversion: 1.9.0 # the version of kubernetes that you want to use
service_cluster_ip_range: 10.32.0.0/24 # the IP range you want to use for the cluster services
service_node_port_range: 30000-32767 # the port range you want to use for the service nodes
cluster_cidr: 10.200.0.0/16 # the ip range you want to use for the cluster
kube_controller_version: 2 # the version you want to use for the controller services
cni_version: "0.3.1" # the version you want to use for CNI
cluster_domain: cluster.local # the domain name you want to use for the cluster
pod_cidr_base: 10.200. # the first 2 octets of your {{ cluster_cidr }}
kube_dns_ip: 10.32.0.10 # the IP address you want to use for the Kube-dns service. This IP must be taken from the subnet you defined in {{ service_cluster_ip_range }}
```
## Dependencies
------------

None

## Example Playbook
----------------
```yaml
    - hosts: kubernetes
      roles:
         - ansible-role-k8scluster
      vars:
        k8sclustername: k8s-test-cluster
        cluster_domain: testenv.local
```
## License
-------

BSD

## Author Information
------------------

This role was created by Erik Christiaans, erik@ascode.nl
