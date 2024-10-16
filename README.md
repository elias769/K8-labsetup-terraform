# Kubernetes Cluster with Kubeadm on AWS using Terraform & Ansible

![Kubernetes AWS Architecture](./img/architecture.png)

---

### Overview
This project automates the creation and configuration of a Kubernetes cluster on AWS using **Terraform** for infrastructure provisioning, **Ansible** for configuration management, and **Kubeadm** for Kubernetes cluster setup.

The solution provides a robust, scalable, and production-ready Kubernetes environment, leveraging AWS resources such as EC2 instances, VPC, and Network Load Balancer (NLB). By integrating automation tools like Terraform and Ansible, you can deploy a Kubernetes cluster with minimal manual intervention.

---

## Tools and Technologies

### Terraform
**Terraform** is an Infrastructure as Code (IaC) tool that allows you to provision and manage infrastructure using code. For this project, Terraform automates the creation of:
- AWS EC2 instances for Kubernetes masters and worker nodes.
- AWS VPC and subnets for networking.
- Security groups, Network Load Balancers, and other infrastructure resources required for a functional Kubernetes cluster.

**Why Terraform?**
- **Automated infrastructure management**: Create, modify, and destroy cloud resources using simple code.
- **State management**: Ensures your infrastructure remains consistent and predictable.
- **Scalability**: Terraform scales with your infrastructure as you add more resources.

### Ansible
**Ansible** is a configuration management tool used to automate server configuration, application deployment, and orchestration. In this project, Ansible:
- Configures the Kubernetes master and worker nodes.
- Sets up Docker (cri-dockerd) and Kubernetes tools (kubeadm, kubelet, kubectl).
- Automates Kubernetes cluster initialization and network setup.

**Why Ansible?**
- **Idempotency**: Ensures that tasks will have the same outcome even if they are run multiple times.
- **Agentless**: No additional software or agent is required on the nodes, making it simple to set up and maintain.
- **Ease of use**: YAML-based playbooks are easy to read, write, and share.

### Kubeadm
**Kubeadm** is a tool provided by Kubernetes to bootstrap and set up a minimum viable Kubernetes cluster. It simplifies cluster initialization by handling the setup of all required components:
- Configures the Kubernetes control plane and joins worker nodes to the cluster.
- Manages the certificate generation, network setup, and installation of essential services.

**Why Kubeadm?**
- **Simplified setup**: Kubeadm abstracts complex setup processes, enabling quick and repeatable deployments.
- **Production-ready**: While lightweight, Kubeadm setups are production-grade and flexible for various infrastructure environments.

### Kubernetes
**Kubernetes** is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. In this project, Kubernetes is deployed across multiple AWS EC2 instances, enabling:
- **Containerized application deployment**.
- **Self-healing**: Kubernetes can automatically restart failed containers and reschedule workloads across healthy nodes.
- **Scaling**: Easily scale applications by adding or removing containers in response to demand.
- **Load balancing**: Efficiently distribute incoming traffic across container instances.

---

## Architecture Diagram
The following diagram illustrates the architecture of the Kubernetes cluster on AWS, including the interaction between various components:

![Kubernetes AWS Architecture](./img/architecture.png)

---

### Prerequisites
- **AWS CLI**: Make sure your [AWS CLI is configured](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).
- **Terraform**: [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) on your machine.
- **Ansible**: [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) for configuration management.

---

## Usage

1. **Clone this repository**:

    ```bash
    git clone https://github.com/your-username/your-repo.git
    cd your-repo
    ```

2. **Review and modify configuration**: Open the `vars.tf` file to adjust the AWS and Kubernetes cluster configurations to your requirements. Default values are already provided, but you can override these via command line or a variable file.

3. **Check AWS region and AMI**: If you're using a different AWS region, ensure you update the `ami_id` variable in `vars.tf`. Use the [Ubuntu AMI Locator](https://cloud-images.ubuntu.com/locator/ec2/) to find the correct AMI for your region.

4. **Deploy Infrastructure**: Initialize and apply the Terraform code to provision the necessary AWS infrastructure.

    ```bash
    terraform init
    terraform plan
    terraform apply
    ```

5. **Kubernetes Cluster Setup**: Once Terraform provisions the infrastructure, Ansible will configure the Kubernetes cluster on top of it. The Bastion host can be accessed with the dynamically created `k8_ssh_key.pem` private key, which is located in the project folder. Use this key to SSH into the Bastion host.

    ```bash
    ssh -i k8_ssh_key.pem ubuntu@<bastion-host-ip>
    ```

---

### Architecture Overview
- **Bastion Host**: Acts as a jump server for accessing and managing the master and worker nodes securely.
- **Master Nodes**: These are the control plane nodes that manage the Kubernetes cluster.
- **Worker Nodes**: These run the actual workloads (containers) in the Kubernetes cluster.
- **AWS NLB**: Load balances traffic across the Kubernetes API server endpoints.




