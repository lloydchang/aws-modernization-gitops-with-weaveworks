+++
title = "Set up credentials"
chapter = true
weight = 20
+++

# Set up credentials

{{% notice warning %}}
Ensure you have the right IAM profile before proceeding with steps

```sh
aws sts get-caller-identity --query Arn | grep modernization-admin -q && echo "IAM role valid" || echo "IAM role NOT valid"
```

{{% /notice %}}

## Kubeconfig for EKS management cluster

* You should see an eks cluster already provisioned when running

```sh
eksctl get clusters
```

* We'll need kubeconfig credentials. You can get it with

```sh
eksctl utils write-kubeconfig --cluster EKS-YOURCLUSTERNAME
```

## AWS Keypair

* Open a new tab and navigate to [AWS console > EC2 > Key Pairs](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#KeyPairs:)
  * Add a new Key Pair named `weaveworks-workshop` and save the pem file
  * In your Cloud9 tab, click File > Upload local files, then choose the Key Pair's pem file that was downloaded. It should have the name `weaveworks-workshop.pem`

## Set up environment variables

* `cd aws-gitops-multicloud`, then run `cp ../gitops-cluster-management/.envrc.example .envrc`
* Open `.envrc` and start populating fields
  * `CAPI_AWS_ACCESS_KEY_ID` to your workshop `AWS_ACCESS_KEY_ID`
  * `CAPI_AWS_SECRET_ACCESS_KEY` to your workshop `AWS_SECRET_ACCESS_KEY`
  * `GIT_USER` to your github username
  * `GIT_REPO_NAME` to your repo name `aws-gitops-multicloud`
  * `AWS_REGION` to `us-west-2`
  * `AWS_SSH_KEY_NAME` to `weaveworks-workshop` that we created earlier
  * we can leave `AWS_CONTROL_PLANE_MACHINE_TYPE` and `AWS_NODE_MACHINE_TYPE` as `t3.large`
* Finally run `direnv allow`. Which will export these env vars whenever you're in the git repo directory.
