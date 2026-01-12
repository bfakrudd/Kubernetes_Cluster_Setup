Setting up a Kubernetes cluster on AWS Cloud using Terraform and Ansible

This guide provides step-by-step instructions to set up a Kubernetes cluster on AWS Cloud using Terraform for infrastructure provisioning and Ansible for configuration management.

Prerequisites
Before you begin, ensure you have the following installed on your local machine:
Terraform (version 0.12 or later)
Ansible (version 2.9 or later)
AWS CLI (configured with your AWS credentials)
kubectl (Kubernetes command-line tool)
An AWS account with necessary permissions to create resources

Step 1: Set up Terraform Configuration
Create a new directory for your project and navigate into it:

mkdir deploy_Ec2_k8s
cd deploy_Ec2_k8s
Create a Terraform configuration file (main.tf) to define the AWS resources needed for the Kubernetes cluster. This includes VPC, subnets, security groups, and EC2 instances.

All the Terraform configuration files are located in the deploy_Ec2_k8s directory.
Step 2: Initialize and Apply Terraform Configuration
Initialize the Terraform configuration:
terraform init
Review the plan and type 'yes' to confirm the creation of resources.
terraform plan -out=myplan
Apply the Terraform configuration to create the resources:
terraform apply myplan
Step 3: Set up Ansible Playbook
Create an Ansible playbook (site.yml) to install and configure Kubernetes on the EC2 instances created by Terraform. The playbook should include tasks to install Docker, kubeadm, kubelet, and kubectl, as well as initialize the Kubernetes cluster and join worker nodes.
All the Ansible playbook files are located in the ansible_k8s_ec2 directory.

Step 4: Run Ansible Playbook
Run the Ansible playbook to configure the Kubernetes cluster:
ansible-playbook -i inventory site.yml

Step 5: Verify Kubernetes Cluster
After the Ansible playbook has completed, verify that the Kubernetes cluster is up and running:
kubectl get nodes
You should see the master and worker nodes listed as ready.

Directory Structure:
.
├── ansible_k8s_ec2
│   ├── ansible.cfg
│   ├── inventory.ini
│   ├── roles
│   │   ├── cni
│   │   │   └── tasks
│   │   │       └── main.yml
│   │   ├── common
│   │   │   └── tasks
│   │   │       └── main.yml
│   │   ├── containerd
│   │   │   └── tasks
│   │   │       └── main.yml
│   │   ├── kubernetes
│   │   │   └── tasks
│   │   │       └── main.yml
│   │   ├── master
│   │   │   └── tasks
│   │   │       └── main.yml
│   │   └── workers
│   │       └── tasks
│   │           └── main.yml
│   └── site.yml
├── deploy_Ec2_k8s
│   ├── linux_command.txt
│   ├── main.tf
│   ├── newplan
│   ├── outputs.tf
│   ├── terraform.tfstate
│   ├── terraform.tfstate.backup
│   ├── terraform.tfvars
│   └── variables.tf
└── README.md

Conclusion
You have successfully set up a Kubernetes cluster on AWS Cloud using Terraform and Ansible. You can now deploy your applications to the cluster and manage them using Kubernetes.



