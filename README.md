# Project8-Demo-Cost-Optimization

## What is AWS Lambda?

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. This is crucial for a DevOps engineer because of the wide range of automation and monitoring tasks that can be event-driven with minimal resource management.

### Key Concepts:
1. **Serverless Architecture**: AWS Lambda follows a serverless architecture, meaning you don’t need to manage underlying infrastructure. AWS automatically provisions and scales the resources required to execute the code.
2. **Event-driven**: Lambda functions are triggered by events from AWS services like S3, CloudWatch, SNS, etc.
3. **Pay-as-you-go**: You are only billed for the compute time used, and there’s no charge when your function is not running.

---

## AWS Lambda vs EC2: Key Differences

### EC2
- **Manual Provisioning**: You have to manually configure your instances (CPU, memory, etc.).
- **Long-running**: EC2 instances are ideal for applications that need persistent resources and long-running processes.
- **Full Control**: You get more control over instance configuration, scaling, and networking.

### AWS Lambda
- **Automatic Scaling**: Resources are automatically managed based on your application’s needs.
- **Short-lived Execution**: Lambda functions are best suited for short-lived tasks and event-driven workloads.
- **No Infrastructure Management**: You don’t have to worry about the underlying servers, OS, or scaling. It’s fully managed by AWS.

For a DevOps engineer, this distinction is important when deciding whether to use Lambda for serverless workloads or EC2 for long-running, more persistent applications.

---

## Why DevOps Engineers Need AWS Lambda

As a DevOps engineer, AWS Lambda offers several use cases that align perfectly with your daily responsibilities, especially when it comes to cost optimization and security compliance.

### 1. **Cost Optimization**
Cost management is a primary responsibility for any DevOps engineer. AWS Lambda, with its serverless architecture, allows you to save on costs by paying only for compute time. This is especially useful for tasks that run intermittently, such as daily monitoring scripts or nightly backups.

- **Automating Cost Monitoring**: You can write Lambda functions to monitor AWS resources for cost inefficiencies, like unused EC2 instances or EBS volumes, and trigger alerts to notify teams.
- **Scheduled Cleanups**: For example, you can create a Lambda function triggered by CloudWatch that checks for unused EBS volumes daily. If the volume is not in use, it could either be deleted or flagged.

### 2. **Security Compliance**
Lambda also plays a significant role in security management. You can use Lambda to automate checks that ensure compliance with your organization’s security policies.

- **Monitoring AWS Resources**: Lambda can automate tasks like checking if any S3 buckets have public access or monitoring for non-compliant EC2 instances or EBS volumes.
- **Automating Notifications**: When non-compliant resources are found, Lambda can trigger notifications through services like SNS, sending alerts to the relevant team.

---
### AWS DevOps Zero to Hero Series: Cloud Cost Optimization using Lambda for Stale Resources

Welcome to Day 18 of the AWS DevOps Zero to Hero series! Today, we’ll dive into **cloud cost optimization**, a critical concept for **DevOps** and **Cloud Engineers** alike. By the end of this article, you'll understand both the theoretical and practical aspects of cloud cost optimization, and we'll also implement a **real-world project** demonstrating how to identify and delete stale AWS resources to manage costs effectively.

---

### Why is Cloud Cost Optimization Important?

Organizations, especially startups and mid-scale companies, often choose to move to the cloud for two primary reasons:
1. **Reducing infrastructure overhead**.
2. **Optimizing infrastructure costs**.

While migrating to the cloud offers flexibility, scaling, and ease of management, there is a hidden risk if the resources are not **managed efficiently**—costs can actually increase. One critical responsibility of a **DevOps Engineer** is ensuring that the cloud environment is optimized to avoid overspending on unused or "stale" resources.

---

Sure! Here's a detailed step-by-step guide based on the provided information about managing EBS snapshots with AWS Lambda for cost optimization:

### Step-by-Step Guide to Manage EBS Snapshots with AWS Lambda

#### **1. Understand Snapshots**
- **Definition**: An EBS snapshot is a point-in-time copy of your Amazon EBS volume.
- **Use Case**: Snapshots can be used for backup purposes. However, unused snapshots can incur unnecessary costs.

#### **2. Set Up Your Environment**
- **AWS Account**: Ensure you have access to an AWS account with appropriate permissions.
- **AWS CLI/Management Console**: Familiarize yourself with the AWS Management Console or AWS CLI for managing EC2 and EBS resources.

#### **3. Create an EC2 Instance and EBS Snapshot**
1. **Launch EC2 Instance**:
   - Navigate to the **EC2 Dashboard**.
   - Click on **Launch Instance**.
   - Choose an Amazon Machine Image (AMI), e.g., Ubuntu, and select an instance type (e.g., `t2.micro`).
   - Configure the instance settings as needed and launch the instance.
  
2. **Create an EBS Snapshot**:
   - After the instance is running, select the instance from the dashboard.
   - Go to the **Volumes** section to find the volume attached to your instance.
   - Select the volume and click on **Create Snapshot**.
   - Provide a name for the snapshot and create it.

#### **4. Write Lambda Function for Snapshot Management**
1. **Create Lambda Function**:
   - Navigate to the **AWS Lambda** service.
   - Click on **Create Function** and choose **Author from Scratch**.
   - Name the function (e.g., `CostOptimizationEBS`) and choose a runtime (e.g., Python).
   - Set permissions as needed (you can use default permissions initially).

2. **Add Function Code**:
   - Copy the provided Python code (from your GitHub repository) that manages EBS snapshots.
   - Paste it into the Lambda code editor.
   - **Save and Deploy** the function.

3. **Increase Execution Timeout**:
   - Go to the **Configuration** tab of your Lambda function.
   - Increase the **Timeout** from the default of 3 seconds to at least 10 seconds.

#### **5. Grant Necessary Permissions**
1. **Identify the Execution Role**:
   - Find the role associated with your Lambda function under the **Permissions** tab.
   - Click on the role to view its permissions.

2. **Add Permissions to the Role**:
   - Click on **Add Permissions** > **Attach Policies**.
   - Create a new policy to include permissions for:
     - `DescribeSnapshots`
     - `DeleteSnapshots`
     - `DescribeInstances`
     - `DescribeVolumes`
   - Attach the policy to your Lambda execution role.

#### **6. Test the Lambda Function**
1. **Create a Test Event**:
   - Click on the **Test** button in the Lambda console.
   - Provide a name for the test event (e.g., `testEvent`) and save it.

2. **Execute the Test**:
   - Click **Test** again to execute the Lambda function.
   - Observe the logs for any errors and ensure it runs successfully.
   - If the function is correctly configured, it should identify and retain snapshots that are associated with active volumes.

#### **7. Automate Snapshot Deletion**
- **Delete EC2 Instance**:
   - After testing, you can delete the EC2 instance from the EC2 dashboard.
   - This will also delete the associated volume, but the snapshot will remain.

- **Run the Lambda Function Again**:
   - Run the Lambda function again to verify that it detects the deletion of the volume and deletes the associated snapshot accordingly.

#### **8. Monitor and Optimize**
- **Implement Notifications**:
   - Consider adding logic to your Lambda function to notify team members if a snapshot is older than a certain threshold (e.g., 30 days) before deletion.

- **Regular Review**:
   - Regularly check for orphaned snapshots and optimize the function as necessary to handle your organization’s specific requirements.



