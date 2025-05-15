# RedHat-OpenShift-UPI-Installation-Cloud-Formation-Templates
Red Hat OpenShift UPI Installation Cloud Formation templates

[Documentation Author: Ankit Jain](https://www.linkedin.com/in/ankitkkjain/)

Here’s a **brief step-by-step guide** to installing a cluster on **user-provisioned infrastructure in AWS** using **CloudFormation templates**:

### **1. Prerequisites**
- Ensure you have an **AWS account** and necessary **IAM permissions**.
- Install **AWS CLI**:
  ```bash
  sudo apt install awscli  # Debian-based systems
  sudo dnf install awscli  # Fedora-based systems
  ```
- Install **OpenShift Installer**:
  ```bash
  curl -O https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/latest/openshift-install-linux.tar.gz
  tar -xvf openshift-install-linux.tar.gz
  ```

### **2. Prepare CloudFormation Templates**
- Download the **CloudFormation templates** from OpenShift documentation.
- Modify the templates to match your **networking and security requirements**.

### **3. Create AWS Infrastructure**
- Create a **VPC**:
  ```bash
  aws cloudformation create-stack --stack-name my-vpc --template-body file://vpc.yaml
  ```
- Set up **networking and load balancers**:
  ```bash
  aws cloudformation create-stack --stack-name my-network --template-body file://network.yaml
  ```
- Configure **IAM roles and security groups**:
  ```bash
  aws cloudformation create-stack --stack-name my-security --template-body file://security.yaml
  ```

### **4. Deploy Cluster Nodes**
- Create the **bootstrap node**:
  ```bash
  aws cloudformation create-stack --stack-name bootstrap --template-body file://bootstrap.yaml
  ```
- Deploy **control plane nodes**:
  ```bash
  aws cloudformation create-stack --stack-name control-plane --template-body file://control-plane.yaml
  ```
- Deploy **worker nodes**:
  ```bash
  aws cloudformation create-stack --stack-name workers --template-body file://workers.yaml
  ```

### **5. Complete Installation**
- Monitor the installation logs:
  ```bash
  openshift-install wait-for bootstrap-complete --log-level=debug
  ```
- Approve **certificate signing requests (CSRs)**:
  ```bash
  oc get csr
  oc adm certificate approve <csr-name>
  ```
- Verify the cluster is running:
  ```bash
  oc get nodes
  ```

For detailed instructions, check out [Red Hat’s official documentation](https://docs.redhat.com/en/documentation/openshift_container_platform/4.13/html/installing_on_aws/installing-aws-user-infra) or [OKD installation guide](https://docs.okd.io/4.17/installing/installing_aws/upi/installing-aws-user-infra.html). Let me know if you need further clarification! 🚀
