# AWS EKS Kubernetes Cluster Example

This example demonstrates the setup of a highly-available AWS EKS Kubernetes cluster using Ansible to manage its setup and teardown.

## Usage

Instructions for EKS are in roles/eks/README.md

Prerequisites:

  - You must have an AWS account to run this example.
  - You must use an IAM account with privileges to create VPCs, Internet Gateways, EKS Clusters, Security Groups, etc.
  - This IAM account will inherit the `system:master` permissions in the cluster, and only that account will be able to make the initial changes to the cluster via `kubectl` or Ansible.
  - If you encounter the following error:
      > Ansible is being run in a world writable directory (/path/to/your/dir), ignoring it as an ansible.cfg source

    you must execute `chmod o-w .`. This is a WSL related problem.
  - Create a vault_password file:

        ---
        aws_access_key: YOUR AWS_ACCESS_KEY
        aws_secret_key: YOUR AWS_SECRET_KEY
    Execute:

      `ansible-vault encrypt vault_password`
  - You must have created a key pair as stated in roles/README.md

You may want to add an overrides.yml file with overriden variables like cluster name, vpc name, vpc cidr, etc. As an example:

    eks_kubeconfig_env:
      - name: AWS_PROFILE
        value: default
    veks_cluster_name: cluster_name
    kube_domain_name: a.domain.name.in.route53

Example commands are shown in demo.txt

### Build the Cluster with Ansible

Make sure you have installed the requirements (`pip3 install requirements.txt`), and then run the playbook to build the EKS cluster and nodegroup:

    $ ansible-playbook -i inventory playbooks/eks.yml -e "<cluster-name>"
    $ ansible-playbook playbooks/eks.yml -vv -e @overrides.yml -e env=test  --ask-vault-pass

> Note: EKS Cluster creation is a slow operation, and could take 10-20 minutes.

After the cluster and nodegroup are created, you should see one EKS cluster and three EC2 instances running, and you can start to manage the cluster with Ansible.

