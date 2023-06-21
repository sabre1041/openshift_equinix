# OpenShift Equinix Collection

Ansible Collection to Deploy OpenShift to the Equinix platform

## Overview

The contents within this collection contain assets to deploy OpenShift to the Equinix platform. The following installation types are supported:

* Single Node with [Assisted Installer](https://docs.openshift.com/container-platform/4.13/installing/installing_on_prem_assisted/installing-on-prem-assisted.html)
* Single Node with [Agent Based Installer](https://docs.openshift.com/container-platform/4.13/installing/installing_with_agent_based_installer/preparing-to-install-with-agent-based-installer.html)

## Installation

Install the required Python packages:

```shell
pip install -r requirements.txt
```

The collection can be installed from Ansible Galaxy:

```shell
ansible-galaxy collection install sabre1041.openshift_equinix
```

## Deploying a Cluster

Use the following steps to deploy an OpenShift cluster to the Equinix platform

### Prerequisites

You will need the following pieces of information:

* Equinix API Token
* Equinix Project ID
* SSH keypair (public/private SSH keys)
* Assisted Installer Offline Token
* Base DNS domain for OpenShift
* OpenShift Cluster Name

### Preparing for Installation

Create a file to container the Ansible variables that are needed to deploy the cluster. You may choose to leverage ansible-vault to protect the sensitive properties, but those steps are outside the scope of this document.

Below is a sample file (called `openshift-equinix-params.yaml`)containing the necessary variables that need to be provided. 

```yaml
openshift_cluster_name: <OPENSHIFT_CLUSTER_NAME>
openshift_base_domain: <OPENSHIFT_BASE_DOMAIN>
openshift_pull_secret_file: "{{ lookup('env', 'PWD') }}/<OPENSHIFT_PULL_SECRET>"

# Metal env
equinix_api_token: <API_TOKEN>
equinix_project_id: <PROJECT_ID>

# Assisted Installer vars
openshift_ai_offlinetoken: <OPENSHIFT_AI_OFFLINE_TOKEN>
ssh_private_key_file: "{{ lookup('env', 'PWD') }}/<SSH_PRIVATE_KEY_PATH>"
ssh_public_key_file: "{{ lookup('env', 'PWD') }}/<SSH_PUBLIC_KEY_PATH>"
```

NOTE: The properties relating to SSH key files and OpenShift Pull Secret refer to a location on the file system relative to the current working directory.

## Deploying a cluster

By default, a Single Node Assisted Installer mode installation is performed. To specify the type, set the `install_type` variable. Valid options are `sno` or `abi`.

Using the variables file created above, execute the following command to deploy OpenShift to Equinix:

```shell
ansible-playbook sabre1041.openshift_equinix.deploy  -e @openshift-equinix-params.yaml
```

Once the installation has completed, create the necessary [DNS records](https://docs.openshift.com/container-platform/4.13/installing/installing_bare_metal/installing-bare-metal.html#installation-dns-user-infra_installing-bare-metal) to associate the newly created instance as per the OpenShift documentation.

Enjoy your OpenShift cluster on Equinix!
