**kubectl **– A command line tool for working with Kubernetes clusters. For more information, see Installing or updating kubectl.
Check if kubectl is installed
Determine whether you already have kubectl installed on your device
`kubectl version --client`

**eksctl** – A command line tool for working with EKS clusters that automates many individual tasks. For more information, see Installing or updating.
check on `https://eksctl.io/installation/`

**AWS CLI** – A command line tool for working with AWS services, including Amazon EKS. For more information, see Installing, updating, and uninstalling the AWS CLI in the AWS Command Line Interface User Guide.
For LInux 
`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install`

After installing the AWS CLI, we recommend that you also configure it. For more information, see Quick configuration with aws configure in the AWS Command Line Interface User Guide.
`https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html`

**Install EKS**

Please follow the prerequisites doc before this.

**Install using Fargate**
`eksctl create cluster --name demo-cluster --region us-east-1 --fargate`
**Delete the cluster**
`eksctl delete cluster --name demo-cluster --region us-east-1`
