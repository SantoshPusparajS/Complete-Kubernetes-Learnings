EKS Control Plane:

- Not shared across the clusters or AWS accounts
- Consists of atleast 2 API server nodes and three ETCD nodes that run across three AZs within a region
- Automatically detects and replaces unhealthy control plane instances, restarting them across the AZs within region

Worker Nodes/Node Groups:

- Worker machines in Kubernetes are call nodes. These are EC2 instances.
- EKS worker nodes run in our account and connect to control plane via API server endpoint
- NodeGroup: one or more EC2 instances that are deployed in EC2 ASG

### COMMANDS

Command to create EKS cluster without any node group:
eksctl create cluster --name=eksdemo --region=us-east-1 --zones=us-east-1a,us-east-1b --without-nodegroup

##Creates a cloudformation under the hood

Command to get the nodes in the cluster:
kubectl get nodes

Command to get the cluster in the region:
eksctl get cluster

Command to attach iam oidc provider to the cluster:
eksctl utils associate-iam-oidc-provider --region us-east-1 --cluster eksdemo --approve

Command to create nodegroups:

# Public Subnet

eksctl create nodegroup --cluster=eksdemo --region=us-east-1 --name=eksdemo-public-ng --node-type=t3.medium --nodes=2 --nodes-min=2 --nodes-max=4 --node-volume-size=20 --ssh-access --ssh-public-key=personalKey --managed --asg-access --external-dns-access --full-ecr-access --appmesh-access --alb-ingress-access

# Private Subnet

eksctl create nodegroup --cluster=eksdemo --region=us-east-1 --name=eksdemo-ng-private1 --node-type=t3.medium --nodes-min=2 --nodes-max=4 --node-volume-size=20 --ssh-access --ssh-public-key=personalKey --managed --asg-access --external-dns-access --full-ecr-access --appmesh-access --alb-ingress-access --node-private-networking

Commad to delete the cluster:
eksctl delete cluster eksdemo

Command to delete the nodegroup:
eksctl delete nodegroup --cluster=eksdemo --name=eksdemo-public-ng

CSI Drivers : Container Storage Interface

1. EBS CSI Driver 2. EFS CSI Driver 3. FSx for Luster CSI
   Not supported for AWS EKS Fargate
   We can dynamically change the configuration of a volume attached to an instance. Recommended for data that must be quickly accessible and requires long-term persistence,

#Command to update the kubeconfig:
aws eks --region us-east-1 update-kubeconfig --name eksdemo
