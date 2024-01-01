#### FARGATE IN AWS EKS

1. Fargate is a serverless compute platform for containers on AWS.
2. EKS integrates K8 with fargate by using controllers that are built by AWS using the upstream,extensible provided by K8s
3. These controllers run as part of EKS managed K8s control plane and are responsible for scheduling native K8s pods onto fargate
4. Fargate controllers include a new scheduler that runs along the default K8 scheduler in addition to several mutating and validating admission controllers
5. When we start the pod that meets the criteria for running on fargate, Fargate controllers running in the cluster recognize, update and schedule the pod onto Fargate.
6. Can bring exisiting workflows and services from K8s to Fargate
7. Each pod runs in a isolated compute environment 1POD == 1 EC2 FARGATE INSTANCES
8. Fargate instances supported only for private subnets only
9. Make it mandatory to include resources.requests and resources.limits about cpu and memory.

# Get list of Fargate Profiles in a cluster

eksctl get fargateprofile --cluster eksdemo

# Creating Fargate profiles

eksctl create fargateprofile --cluster eksdemo --name fp-demo --namespace fargate

# Will create fargatepodexecutionrole as a part of this

# Deleting fargate profiles in eks cluster

eksctl delete fargateprofile --cluster eksdemo1 --name fp-demo --wait
