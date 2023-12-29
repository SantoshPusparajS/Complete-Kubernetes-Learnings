##EBS CSI DRIVER INSTALLATION

1. Create a policy and attach it to the role of AWS EKS worker nodes.

{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": [
"ec2:AttachVolume",
"ec2:CreateSnapshot",
"ec2:CreateTags",
"ec2:CreateVolume",
"ec2:DeleteSnapshot",
"ec2:DeleteTags",
"ec2:DeleteVolume",
"ec2:DescribeInstances",
"ec2:DescribeSnapshots",
"ec2:DescribeTags",
"ec2:DescribeVolumes",
"ec2:DetachVolume"
],
"Resource": "\*"
}
]
}

2. Attach the policy to the worker nodes role
3. Install AWS EBS CSI Driver
   kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=master"

Command to get the role arn of worker nodes:
kubectl -n kube-system describe configmap aws-auth

Command to connect to mysql:
kubectl run -it --rm --image=mysql:latest --restart=Never mysql-client -- mysql -h mysql -pSan210996
