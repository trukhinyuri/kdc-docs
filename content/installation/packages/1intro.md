---
title: Kubernetes Variables
linktitle: Variables
description: Description of variables that will be used throughout the installation and network configuration info.

categories: []
keywords: []

menu:
  docs:
    parent: "packages"
    weight: 2
    identifier: variables

draft: false
---

# Before you start
Before you start bootstrapping a cluster with Containerum Kubernetes Package be sure to read through the variables that will be used throughout this installation guide. Also don't forget to configure the network and set Containerum repo in advance as described below.

## Variables  
### IP addresses

- `KUBERNETES_PUBLIC_IP` is an IP address of Kubernetes load balancer in a public network. In the case of only one node it is equal to the master node's `EXTERNAL_IP` value
<!-- - `PUBLIC_IP` is equal to `KUBERNETES_PUBLIC_IP` -->
- `EXTERNAL_IP` is an IP address of an instance in external network
- `INTERNAL_IP` is an IP address of instance in internal network
- `MASTER_NODES_IP` is a sequence of all IP addresses of master nodes. In the case of only one node it is equal to the master node's `EXTERNAL_IP` value
- `ETCD_NODE_IP` is an IP address of the etcd node. In case of multiple etcd nodes they can be declared as `ETCD_NODE_1_IP`, `ETCD_NODE_2_IP`, etc.
- `POD_CIDR` is the range of IP addresses for pods

### Hostnames

- `HOSTNAME` is the hostname of the node
- `NODE_NAME` is the name of the node. In most cases it is equal to `HOSTNAME`
- `ETCD_NAME` is the hostname of the instance, on which etcd has been installed

## Network information

It is necessary to ensure that all cluster hosts can communicate by hostname. It will be sufficient to add the following entries to /etc/hosts on each node:  
192.168.0.4 master  
192.168.0.5 node-1  
192.168.0.6 node-2  

Set a separate hostname for each node. For the node with the master role and name set:
```bash
hostnamectl set-hostname master
```
Do the same for the worker nodes `node-1` and `node-2`.

Configure the network interfaces for public and private networks:

- public eth0:

```
BOOTPROTO=none
DEFROUTE=yes
DEVICE=eth0
GATEWAY=192.168.0.1
IPADDR=192.168.0.2
MTU=1500
NETMASK=255.255.255.0
ONBOOT=yes
TYPE=Ethernet
USERCTL=no
```

- private eth1:

```
BOOTPROTO=none
DEVICE=eth1
IPADDR=10.0.10.1
MTU=1500
NETMASK=255.255.255.0
ONBOOT=yes
TYPE=Ethernet
USERCTL=no
```

## Containerum RPM repository

### Repository definition

Put this in /etc/yum.repos.d/exonlab.repo:
```
[exonlab-kubernetes110-testing]
name=Exon lab kubernetes repo for CentOS
baseurl=http://repo.containerum.io/centos/7/x86_64/
skip_if_unavailable=False
gpgcheck=1
repo_gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ExonLab
enabled=1
enabled_metadata=1

```

### GPG package signing key

```
curl -O http://repo.containerum.io/RPM-GPG-KEY-ExonLab
sudo mv RPM-GPG-KEY-ExonLab /etc/pki/rpm-gpg/
sudo chown root:root /etc/pki/rpm-gpg/RPM-GPG-KEY-ExonLab
```

Key fingerprint: `2ED4 CBD2 309F 2C75 1642  CA7B 4E39 9E04 3CDA 4338`

Now you can proceed to [configuring certificates.](/installation/packages/2certificates)