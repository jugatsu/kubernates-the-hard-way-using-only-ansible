# Description

This repository contains ansible playbooks for automating all steps of [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/README.md) tutorial.

> Install and configure The Google Cloud SDK https://cloud.google.com/sdk/downloads.

## Pre-Requirements

Generate ssh keys:
```bash
ssh-keygen -f ./files/ssh/k8s_the_hard_way -P "" -C k8s
```

### Configure ansible GCE modules

Copy example gce configuration:
```bash
cp .env.gce.example .env.gce
```
Edit according to your environment and then export all variables:
```bash
export $(cat .env.gce)
```

### Provision GCE infrastructure
```bash
ansible-playbook playbook.yml --tags infra
```

### Configure ansible GCE dynamic inventory
Copy example `gce.ini` file:
```bash
cp ./inventory/gce.ini.example ./inventory/gce.ini
```
Edit according to your environment and then check that dynamic inventory is configured properly:
```bash
ansible -m ping all
```
> For full documentation see http://docs.ansible.com/ansible/latest/guide_gce.html

### Run ansible playbook for bringing up the Kubernetes cluster:
```bash
ansible-playbook playbook.yml
```

### Verify that cluster is working
```bash
kubectl get cs
kubectl get no
```

## Cleanup
```bash
ansible-playbook 14-cleanup.yml
kubectl config delete-context kubernetes-the-hard-way
kubectl config delete-cluster kubernetes-the-hard-way
```
