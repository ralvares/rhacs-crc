# DISCLAIMER 

This environment has been created for the sole purpose of providing an easy to deploy and consume a Red Hat Advanced Cluster Security environment as a sandpit.

**Use it at your own please and risk!**

# Install Red Hat Advanced Cluster Security for Kubernetes on CRC - Red Hat CodeReady Containers
Deploy Red Hat Advanced Cluster Security for Kubernetes Demo ( Apps and Pipelines ) on OpenShift 4.x or CRC in a easy and automated way.

## PREREQUISITES

### Red Hat CodeReady Containers
[Red Hat CodeReady Containers](https://developers.redhat.com/products/codeready-containers/overview)

#### Minimum Requirements to run the demo workload on top of CRC: [Configuring the virtual machine](https://code-ready.github.io/crc/#configuring-the-virtual-machine_gsg)

- Minimum 4 vCPU (additional are strongly recommended).
- Minimum 16 GB RAM (additional memory is strongly recommended).

### Helm
[Helm Quick Start](https://helm.sh/docs/intro/quickstart/)

### Ansible 2.9
- [Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) 
- PyYAML
- python3-openshift
- python3-jmespath


### Define variables for your cluster

| Variable | Description |
| -------- | -------- | 
| ocp_username    | OpenShift Cluster Admin User  | 
| ocp4_workload_stackrox_central_admin_password    | RHACS - Central Admin Password     | 
| ocp4_workload_stackrox_central_orchestrator    |  crc or openshift ( full cluster )     |
| ACTION    | create ( To install ) or destroy ( To uninstall )  | 


### Steps

#### Configuring the CRC Virtual Machine
---
```
crc config set cpus 4
crc config set memory 16384
```

#### Installing RHACS and Demo workloads 
---
```
pip
ansible-galaxy collection install kubernetes.core
git clone https://github.com/ralvares/rhacs-crc
cd rhacs-crc
```

You can use the **rhacs-install.yaml** as example, please change the credentials before running the playbook.

rhacs-install.yaml file Example
---
```
- hosts: localhost
  vars:
    ocp_username: kubeadmin
    ocp4_workload_stackrox_central_admin_password: QHb7jH9SusmBmmQX 
    ocp4_workload_stackrox_central_orchestrator: crc
    #ocp4_workload_stackrox_central_orchestrator: openshift
  roles:
  - ocp4_workload_stackrox_central
  - ocp4_workload_stackrox_sensor
  - ocp4_workload_stackrox_demo_apps
  - ocp4_workload_stackrox_demo_pipeline
```

Login to crc/ocp using a Cluster-Admin user.
---
```
oc login -u kubeadmin https://api.crc.testing:6443 
```

Running the playbook.
---
```
ansible-playbook rhacs-install.yaml
```

It might take a bit of time, so grab a coffee and enjoy :)


