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


