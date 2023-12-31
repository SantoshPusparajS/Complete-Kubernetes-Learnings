Ingress: exposes HTTP and HTTPS routes from outside the cluster to the services within the cluster.Traffic routing is controlled by the rules defined on the ingress resource.

ALB Ingress Controller: triggers the creation of ALB and necessary supporting AWS resources whenever and Ingress resource is created on cluster with
kubernetes.io/ingress.class: alb notation

ALB Ingress Controller is renamed and redesigned to AWS Load Balancer Controller
Useful only for AWS ALB Useful to create both ALB and NLB
Deprecated Supports latest versions

1. IAM policy is attached to IAM role.
2. Annotate the IAM role with the AWS Load Balancer Controller SA (kube-system-ns)
3. LB controller deployment creates AWS LBC webhook cluster IP service and secret(kube-system-ns)
4. Service account created is associated with the deployment so it can able to create services in AWS.
5. AWS LB Controller will watch for the ingress events in K8s API Server(deployment manifests using kubectl) and creates resources in AWS.

Supports two traffic modes: Instance and IP

\*Instance mode: (Default mode and used for managed group)

1. Registers the nodes within your cluster as targets for the ALB.
2. Traffic reaching the ALB is routed to NodePort for service and proxied to your pods.
3. Explicitly specify using the annotation: alb.ingress.kubernetes.io/target-type: instance

\*IP mode: (Useful for fargate)

1. Register pods as targets for the ALB and traffic reaching ALB is directly routed to the pods for your service.
2. Must specify the annotationns: alb.ingress.kubernetes.io/target-type: ip

# Download IAM Policy

## Download latest

curl -o iam_policy_latest.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json

# Create IAM Policy using policy downloaded

aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy_latest.json

# Verify if any existing service account

kubectl get sa -n kube-system
kubectl get sa aws-load-balancer-controller -n kube-system

# Use eksctl command to create iam service account

# Template

# This will create K8 SA and IAM role. In addition to that it will annotate the IAM role in the SA

eksctl create iamserviceaccount --cluster=eksdemo --namespace=kube-system --name=aws-load-balancer-controller --attach-policy-arn=arn:aws:iam::022071580777:policy/AWSLoadBalancerControllerIAMPolicy --override-existing-serviceaccounts --approve

    eksctl get iamserviceaccount --cluster eksdemo

## Replace Cluster Name, Region Code, VPC ID, Image Repo Account ID and Region Code

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=eksdemo --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller --set region=us-east-1 --set vpcId=vpc-011696c28c4a3b108 --set image.repository=602401143452.dkr.ecr.us-east-1.amazonaws.com/amazon/aws-load-balancer-controller

# Verify that the controller is installed.

kubectl -n kube-system get deployment
kubectl -n kube-system get deployment aws-load-balancer-controller
kubectl -n kube-system describe deployment aws-load-balancer-controller

# Uninstalling AWS Load Balancer Controller

helm uninstall aws-load-balancer-controller -n kube-system

Need for Ingress Class ?

- Many ingress controllers like AWS EKS ALB IC, NgInx IC, AKS AG IC. To identify which ingress controller our Ingress resource/service should be associated to.
- Kubernetes Object -> kind: IngressClass (tied to our controller)

Ingress Manifests:

Ingress Annotations: LoadBalancer settings

Nodeport spans accross all the nodes in the cluster

Path types:
ImplementationSpecific: Controller implementing specific
Exact: Matches the url path exactly and case sensitive
Prefix: Matches based on URL path prefix split by /. Matching is case sensitive and done on path element basis.
