Export the Worker Role Name for use throughout the workshop: 

```
STACK_NAME=$(eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName')
ROLE_NAME=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME | jq -r '.StackResources[] | select(.ResourceType=="AWS::IAM::Role") | .PhysicalResourceId')
echo "export ROLE_NAME=${ROLE_NAME}" | tee -a ~/.bash_profile
```

Kubernetes Dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

kubectl proxy --port=8080 --address='0.0.0.0' --disable-filter=true &

In your Cloud9 environment, click Tools / Preview / Preview Running Application

Scroll to the end of the URL and append:

/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

Generate Token

aws eks get-token --cluster-name eksotguswest2 | jq -r '.status.token'

Ensure that the ELB Service Role Exists else create it

aws iam get-role --role-name "AWSServiceRoleForElasticLoadBalancing" || aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"

Install Helm CLI

```
cd ~/environment

curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh

chmod +x get_helm.sh

./get_helm.sh
```

Configure Helm access with RBAC: Helm relies on a service called tiller that requires special permission on the kubernetes cluster, so we need to build a Service Account for tiller to use. We’ll then apply this to the cluster.

To create a new service account manifest:

```
cat <<EoF > ~/environment/rbac.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EoF
```
```
kubectl apply -f ~/environment/rbac.yaml
```
Then we can install tiller using the helm tooling

```
helm init --service-account tiller
```
This will install tiller into the cluster which gives it access to manage resources in your cluster.

Activate helm bash-completion

```
helm completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
```

