# Getting Started with Amazon ECS Using Amazon EC2<a name="getting-started-ecs-ec2"></a>

Get started with Amazon Elastic Container Service \(Amazon ECS\) using the EC2 launch type by creating a task definition, scheduling tasks, and configuring a cluster in the Amazon ECS console\. For more information, see [Amazon ECS Launch Types](launch_types.md)\.

In the Regions that don't support AWS Fargate, the Amazon ECS first\-run wizard guides you through the process of getting started with tasks that use the EC2 launch type\. The wizard gives you the option of creating a cluster and launching a sample web application\. If you already have a Docker image to launch in Amazon ECS, you can create a task definition with that image and use that for your cluster instead\.

**Important**  
For information about the Amazon ECS first\-run wizard for Fargate tasks, see [Getting Started with Amazon ECS using Fargate](getting-started-fargate.md)\.

You can optionally create an Amazon Elastic Container Registry \(Amazon ECR\) image repository and push an image to it\. For more information, see the *[Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)*\.

Complete the following tasks to get started with Amazon ECS:

## Prerequisites<a name="first-run-ec2-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) and that your AWS user has either the permissions specified in the `AdministratorAccess` or [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.

The first\-run wizard attempts to automatically create the Amazon ECS service IAM and container instance IAM role\. To ensure that the first\-run experience is able to create these IAM roles, one of the following must be true:
+ Your user has administrator access\. For more information, see [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
+ A user with administrator access has manually created these IAM roles so that they are available on the account to be used\. For more information, see [Service Scheduler IAM Role](ecs-legacy-iam-roles.md#service_IAM_role) and [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\. 

## Step 1: Create a Task Definition<a name="first-run-ec2-task-def"></a>

A task definition is like a blueprint for your application\. Each time that you launch a task in Amazon ECS, you specify a task definition\. The service then knows which Docker image to use for containers, how many containers to use in the task, and the resource allocation for each container\.

1. Open the Amazon ECS console first\-run wizard at [https://console\.aws\.amazon\.com/ecs/home\#/firstRun](https://console.aws.amazon.com/ecs/home#/firstRun)\.

1. From the navigation bar, select the **South America \(Sao Paulo\)** Region\.

1. Configure your task definition parameters\.

   The first\-run wizard comes preloaded with a task definition named `console-sample-app-static`, and you can see the `simple-app` container defined in the console\. You can optionally rename the task definition or review and edit the resources used by the container \(such as CPU units and memory limits\)\. Choose the container name and editing the values shown \(CPU units are under the **Advanced options** menu\)\. Task definitions created in the first\-run wizard are limited to a single container for simplicity\. You can create multi\-container task definitions later in the Amazon ECS console\.

   For more information about what each of these task definition parameters does, see [Task Definition Parameters](task_definition_parameters.md)\.
**Note**  
If you are using an Amazon ECR image in your container definition, be sure to use the full `registry/repository:tag` naming for your Amazon ECR images\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest`\.

1. Choose **Next step**\.

## Step 2: Configure the Service<a name="first-run-ec2-service"></a>

In this section of the wizard, you select how you would like to configure the Amazon ECS service that is created from your task definition\. A service launches and maintains a specified number of copies of the task definition in your cluster\. The `simple-app` application is a web\-based Hello World–style application that is meant to run indefinitely\. By running it as a service, it restarts if the task becomes unhealthy or unexpectedly stops\.

The first\-run wizard comes preloaded with a service definition, and you can see the `sample-webapp` service defined in the console\. You can optionally rename the service or review and edit the details by doing the following:

1. For **Service name**, select a name for your service\.

1. For **Desired number of tasks**, enter the number of tasks to launch with your specified task definition\.

1. \(Optional\) You can choose to use an Application Load Balancer with your service\. When a task is launched from a service that is configured to use a load balancer, the task is registered with the load balancer\. Traffic from the load balancer is distributed across the instances in the load balancer\. For more information, see [Introduction to Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)\.
**Important**  
Application Load Balancers do incur cost while they exist in your AWS resources\. For more information, see [Application Load Balancer Pricing](http://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/pricing/)\.

   1. Choose the **Application Load Balancer listener port**\. The default value here is set up for the sample application, but you can configure different listener options for the load balancer\. For more information, see [Service Load Balancing](service-load-balancing.md)\.

   1. In the **Application Load Balancer target group name** field, specify a name for the target group\.

1. Review your service settings and choose **Next step**\.

## Step 3: Configure the Cluster<a name="first-run-ec2-cluster"></a>

In this section of the wizard, you configure your cluster\. Then, Amazon ECS takes care of the networking and IAM configuration for you\.

1. For **Cluster name**, choose a name for your cluster\.

1. For **EC2 instance type**, choose the instance type to use for your container instances\. Instance types with more CPU and memory resources can handle more tasks\. For more information about the different instance types, see [Amazon EC2 Instances](http://aws.amazon.com/ec2/instance-types/)\.

1. For **Number of instances**, type the number of Amazon EC2 instances to launch into your cluster for task placement\. The more instances you have in your cluster, the more tasks you can place on them\. Amazon EC2 instances incur costs while they exist in your AWS resources\. For more information, see [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing/)\.

1. Select a key pair name to use with your container instances\. This is required for you to log into your instances with SSH\. If you do not specify a key pair here, you cannot connect to your container instances with SSH\. If you do not have a key pair, you can create one in the Amazon EC2 console\. For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

1. \(Optional\) In the **Security Group** section, you can choose a CIDR block that restricts access to your instances\. The default value \(**Anywhere**\) allows access from the entire internet\.

1. In the **Container instance IAM role** section, choose an existing Amazon ECS container instance \(`ecsInstanceRole`\) role that you have already created, or choose **Create new role** to create the required IAM role for your container instances\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Choose **Review & launch**\.

## Step 4: Review<a name="first-run-ec2-review"></a>

1. Review your task definition, task configuration, and cluster configurations and click **Launch instance & run service** to finish\. You are directed to a **Launch status** page that shows the status of your launch\. It describes each step of the process \(this can take a few minutes to complete while your Auto Scaling group is created and populated\)\.

1. After the launch is complete, choose **View service**\.

## Step 5: \(Optional\) View your Service<a name="first-run-ec2-view"></a>

If your service is a web\-based application, such as the `simple-app` application, you can view its containers with a web browser\.

1. On the **Service: *service\-name*** page, choose **Tasks**\.

1. Choose a task from the list of tasks in your service\.

1. In the **Containers** section, expand the container details\. In the **Network bindings** section, for **External Link** you will see the **IPv4 Public IP** address to use to access the web application\.

1. Enter the **IPv4 Public IP** address in your web browser and you should see a webpage that displays the **Amazon ECS sample** application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Sample_Application.png)

## Step 6: Clean Up<a name="first-run-ec2-cleanup"></a>

When you are finished using an Amazon ECS cluster, you should clean up the resources associated with it to avoid incurring charges for resources that you are not using\.

Some Amazon ECS resources, such as tasks, services, clusters, and container instances, are cleaned up using the Amazon ECS console\. Other resources, such as Amazon EC2 instances, Elastic Load Balancing load balancers, and Auto Scaling groups, must be cleaned up manually in the Amazon EC2 console or by deleting the AWS CloudFormation stack that created them\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.
**Note**  
If your cluster has registered container instances, you must deregister or terminate them\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\.

1. Choose **Delete Cluster**\. At the confirmation prompt, enter **delete me** and then choose **Delete**\. Deleting the cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.