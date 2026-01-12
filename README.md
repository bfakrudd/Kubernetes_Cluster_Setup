☸️ Kubernetes Cluster on AWS (Terraform & Ansible)

This guide provides a comprehensive workflow for provisioning and configuring a Kubernetes cluster on AWS. We use Terraform for Infrastructure as Code (IaC) and Ansible for automated configuration management.
📋 Prerequisites

Before you begin, ensure your local environment has the following:

    Terraform (≥ 0.12)

    Ansible (≥ 2.9)

    AWS CLI (Configured with aws configure)

    kubectl (The Kubernetes command-line tool)

    IAM Permissions: Ensure your AWS user has rights to manage VPCs, EC2s, and Security Groups.


🛠 Step-by-Step Setup
1. Initialize Infrastructure (Terraform)

Navigate to the Terraform directory to define your VPC, subnets, and EC2 instances.

<pre>
mkdir deploy_Ec2_k8s
cd deploy_Ec2_k8s
# Place your main.tf, variables.tf, and outputs.tf here
</pre>


Run the following commands to provision the hardware:
<pre>
terraform init              # Initialize provider plugins
terraform plan -out=myplan   # Review the resource creation plan
terraform apply "myplan"     # Execute the plan to create AWS resources       
</pre>


2. Configure the Nodes (Ansible)

Once the EC2 instances are running, use Ansible to install the Kubernetes runtime.
<pre>
cd ../ansible_k8s_ec2
ansible-playbook -i inventory.ini site.yml
</pre>
The playbook automates:

    Installing Docker/Containerd.

    Installing kubeadm, kubelet, and kubectl.

    Initializing the Master node.

    Joining Worker nodes to the cluster.

3. Verify the Cluster

Finalize the setup by checking the status of your nodes from your local machine:
Bash
kubectl get nodes
This command should list all nodes in the cluster with their status as "Ready."

Project Structure
<pre>
.
├── ansible_k8s_ec2/           # Ansible configuration files
│   ├── inventory.ini          # IP addresses of AWS instances
│   ├── site.yml               # Main playbook
│   └── roles/                 # Modular configuration roles
│       ├── common/            # Shared dependencies
│       ├── containerd/        # Container runtime
│       ├── kubernetes/        # K8s binaries
│       ├── master/            # Control plane initialization
│       └── workers/           # Node join logic
├── deploy_Ec2_k8s/            # Terraform infrastructure files
│   ├── main.tf                # AWS Resource definitions
│   ├── variables.tf           # Variable declarations
│   └── outputs.tf             # Output values (IPs, etc.)
└── README.md
</pre>
Conclusion

You now have a production-ready foundation for a Kubernetes cluster. From here,you can begin deploying your containerized applications.
