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
