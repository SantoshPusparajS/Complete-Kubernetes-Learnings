Need for externaldns ?

With externaldns we can automatically add AWS Route53 dns record for K8 Ingress service or K8 Service
by defining it as Annotation.
Adds automatically in the AWS Route53 dns records

1. Create IAM policy
2. Create iamservice associating with IAM policy created
   This will create IAM role and K8s service account
3. Deploy ExTERNALdns MANIFEST
