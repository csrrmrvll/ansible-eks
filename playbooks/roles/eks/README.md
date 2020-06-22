# eks

The eks role creates an EKS cluster with associated worker nodes

## Requirements

### Linked service role
Currently there is no way to create service linked roles with Ansible

    aws iam create-service-linked-role --aws-service-name=eks.amazonaws.com

### key pair
Reasonable defaults are set for most values but in particular, the file ~/.ssh/{{ eks_key_file }}.pub must exist. eks_key_file defaults to the value of eks_cluster_name, by default `bsab-eks-test`. Either create an ssh key pair to match these defaults or override the value of eks_key_file to match an existing key. This file is then used to create a key called eks_key_name, which defaults to the value of eks_key_file.

### botocore
botocore must be 1.10.32 or above. Upgrading AWS CLI or boto3 will both upgrade botocore.

### kubectl
kubectl must be 1.10.0+ to work with aws-iam-authenticator

### aws-iam-authenticator
Install the aws-iam-authenticator using the instructions at https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html#eks-prereqs

### Amazon EKS-optimized AMI ids
To get EKS-optimized AMI ids run this command with the proper region code and override the variable veks_ami_id accordingly:

    $ aws ssm get-parameter --name /aws/service/eks/optimized-ami/1.16/amazon-linux-2/recommended/image_id --region region-code --query "Parameter.Value" --output text

### ingress-nginx
Enabled by default, a Route53 hosted zone must be created for it.

### openshift library
Very very bleeding edge right now (should be fixed soonish)

    · openshift from https://github.com/openshift/openshift-restclient-python/pull/196
    · kubernetes python client with base updated to https://github.com/kubernetes-client/python-base/pull/75/files
Use pip install -r requirements.txt to install suitable versions

## Role Variables

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

## Dependencies

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

## Example Playbook

    - hosts: localhost
      connection: local

      roles:
        - eks

## License

BSD

## Author Information

cromero@atsistemas.com
