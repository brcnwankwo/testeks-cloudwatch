
## Deploy an EKS cluster using eksctl and view monitoring insights on CloudWatch.

---

#### What is eksctl?

eksctl is a simple CLI tool for creating and managing clusters on EKS - Amazon's managed Kubernetes service for EC2.

![](![[eksctl.png]].png)




### Prerequisites:

-   Basic knowledge of AWS & eksctl
-   AWS account
-   AWS Access & Secret Key

In this project I will create an EKS cluster named `testeks` using `version 1.24` in region `us-east-1` with the node group name `standard-workers` with an instance type of `t3.micro` and 2 nodes with an auto scaler from 1 to 2 nodes depending on use.

**Step 1: Open terminal and run command**

`eksctl create cluster --name testeks --version 1.24 --region us-east-1 --nodegroup-name standard-workers --node-type t3.micro --nodes 2 --nodes-min 1 --nodes-max 2`


![](![[Screenshot%202022-12-20%20at%2011.01.13%20AM.png]].png)

- Two CloudFormation stacks will be created for the cluster.

	![](![[Screenshot%202022-12-20%20at%2011.09.49%20AM.png]].png)

- Two t3.micro instances were displayed in the us-east-1 region.

	![](![[Screenshot%202022-12-20%20at%2011.09.28%20AM.png]].png)

**Step 2: Attach CloudWatch Log policy to IAM role**

- Click on the IAM role within the instance

	![](![[Screenshot%202022-12-20%20at%2011.08.02%20AM%201.png]].png)

- Attach the CloudWatchLogFullAccess policy to the IAM role to get monitoring access for the cluster.

	![](2022-12-27-00-18-26.png)

**Step 3: Attach CloudWatch agent to cluster**

- Install the CloudWatch agent to the cluster for monitoring.

`curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/testeks/;s/{{region_name}}/us-east-1/" | kubectl apply -f -`

![](![[Screenshot%202022-12-20%20at%2011.10.30%20AM.png]].png)

**Step 4: View insights within CloudWatch**
- Go to Container Insights within CloudWatch to see the various metrics on the cluster.

	![](![[Screenshot%202022-12-20%20at%2011.16.41%20AM.png]].png)
	![](![[Screenshot%202022-12-20%20at%2011.16.04%20AM.png]].png)
	![](![[Screenshot%202022-12-20%20at%2011.16.20%20AM%201.png]].png)

- Within CloudWatch go to Log Groups to view logs within the cluster 

	![](![[Screenshot%202022-12-20%20at%2011.16.56%20AM.png]].png)
	![](![[Screenshot%202022-12-20%20at%2011.18.29%20AM.png]].png)
	![](![[Screenshot%202022-12-20%20at%2011.18.53%20AM%201.png]].png)
	![](![[Screenshot%202022-12-20%20at%2011.19.58%20AM.png]].png)

