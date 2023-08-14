Links:

https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/cloudprovider/aws/README.md

https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html

 Configure Amazon EKS Auto Scaling
https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html

1. Create cluster

eksctl create cluster -f eks-cluster.yaml --profile private

2. Create iam-policy for autoscaller

3. Create identity provider for k8s cluster

-------------- Creating an IAM OIDC provider for your cluster -----------------
# https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html

export cluster_name=nebotask
oidc_id=$(aws eks describe-cluster --profile private --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)

aws iam list-open-id-connect-providers --profile private | grep $oidc_id | cut -d "/" -f4
eksctl utils associate-iam-oidc-provider  --profile private --cluster $cluster_name --approve
------------------------------------------------------------------------


4. Create IAM role (type Web identity)  and attach policy for autoscaller







Clean up:

kubectl delete -f ./k8s/

eksctl delete cluster -f eks-cluster.yaml --profile private