`# Aws_Projects
In this repo there will be all aws files are available

#Apache2_Configuration_File

Add this to the apache2 sites-enable file system 

<Directory /media/myserver/>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
    </Directory>



# EC2_Instances

1.security is the primary concern of the aws thats why ec2 service is provide the security to the user in the aws 

2.it provide ec2 is a secure and resizable compute capacity in the cloud 

2. as per the name of elastic it is the resizable service as per the users requirements .

3.as per the requirement we can scale up the capacity 

4. it provides the facility to the developer 

5.kabhi kabhi bohot jyada requirements agayi at  that time aap use apke company mein scale up kar sakte ho.

6. we can scale up the instances to handle the requirements , demands and requests.

7. as per the requirment user can scale up and scale down the instances 

8. no of instance can be scale up and scale down as per the requirements as per the demand of the server .

9. all is in hand of the user 

10. integrated with the other services example s3 and RDS

11. pay for what you use 

12. users can decide the usage for the usage 

13. on hourly basis price is set 

14. instances can launch in more regions and the availability zones .

15. regions can compose of more than one availability zones 

16. and availabilty zones can consist of one or more distinct datac centers

17. there is a choice of user can choose a different type of operating system and it provides a flexibility

18. for the using the resources we need a secure network 

19. it is working with Amazon VPC to provide the secure network 

20. It supports different operating system 


what is AMI ?

1. In AWS (Amazon Web Services), AMI stands for Amazon Machine Image.

2. An AMI is a pre-configured template used to create virtual servers, known as instances,
    within the Amazon Elastic Compute Cloud (EC2) environment.


   
what is EBS ?

1.In AWS (Amazon Web Services), EBS stands for Elastic Block Store.
2.It is a scalable block storage service designed for use with Amazon EC2 instances. 
3. EBS provides persistent storage volumes that can be attached to EC2 instances, 
 offering durable and reliable storage for data and applications.


 # Creating_Simple_Ec2_Instance_Steps

 to create a simple ec2 instance and getting the access in the terminal of windows server and the mobaxterminal 

1. copy the ssh path and paste it in the terminal 

1. before paste it in the terminal make sure that you are on the location of key which is use in instance 

2. after the key location paste the ssh path and access it 

3. after the access switch in the root terminal command is (sudo su ) for the mobaxterminal .

for the windows users the command is (ssh -i (key name with pem extension) ubuntu@ec2-51-20-135-124.eu-north-1.compute.amazonaws.com )

4. to check the machine operating system ( cat /etc/os-release)

5. to check the how many storage we get use (free -m)command .

6. to check the how many cpu we get then use the lscpu command .

7. to check the machine storage then use the df -h command it configure the




# AWS_Lambda

AWS LAMBDA


AWS Lambda is a serverless computing service provided by Amazon Web Services (AWS).

It allows you to run code without provisioning or managing servers. 

With Lambda, you can upload your code in the form of functions,
 and AWS will automatically handle the infrastructure required to execute that code.


features

Serverless Computing: You don't need to worry about managing servers, scaling, or 
provisioning resources. AWS Lambda automatically handles these tasks for you.


Pay-Per-Use Pricing: With AWS Lambda, you only pay for the compute time consumed by your functions.
 There are no upfront costs or charges when your code is not running.

Support for Multiple Languages: Lambda supports several programming languages, 
including Python, Node.js, Java, C#, and Go. 
This allows you to use the language of your choice to develop your serverless applications.


To Stop Instance
```
import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
```

To Start The Instance

```import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances))
	
```


# How do I use Lambda to stop and start Amazon EC2 instances at regular intervals?


.....Short description
Use AWS Lambda and Amazon EventBridge to automatically stop and start Amazon EC2 instances.

Note: The following resolution is a simple example solution. For a more advanced solution, use the AWS Instance Scheduler. For more information, see Automate starting and stopping AWS instances.

To use Lambda to stop and start EC2 instances at regular intervals, complete the following steps:

1.Create a custom AWS Identity and Access Management (IAM) policy and IAM role for your Lambda function.
2.Create Lambda functions that stop and start your EC2 instances.
3.Test your Lambda functions.
4.Create EventBridge schedules that run your function on a schedule.

Note: You can also create rules that react to events in your AWS account.
Resolution
Note: After you complete the following steps, you might receive a Client error on launch error. For more information, see When I start my instance with encrypted volumes attached, the instance immediately stops with the error "client error on launch."

Get the IDs of the EC2 instances that you want to stop and start. Then, complete the following steps.

Create an IAM policy and IAM role for your Lambda function
Use the JSON policy editor to create an IAM policy. Paste the following JSON policy document into the policy editor:
```
{  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Start*",
        "ec2:Stop*"
      ],
      "Resource": "*"
    }
  ]
}
```
Create an IAM role for Lambda.
Important: When you attach a permissions policy to Lambda, make sure that you choose the IAM policy.

Note: If you use an Amazon Elastic Block Store (Amazon EBS) volume that's encrypted by a customer-managed AWS Key Management Service (AWS KMS) key, then add kms:CreateGrant to the IAM policy.

1.Create Lambda functions that stop and start your instances
2.Open the Lambda console, and then choose Create function.
3.Choose Author from scratch.
Under Basic information, enter the following information:

For Function name, enter a name that describes the function, such as "StopEC2Instances".
For Runtime, choose # Python 3.9.
Under Permissions, expand Change default execution role.
Under Execution role, choose Use an existing role.
Under Existing role, choose the IAM role.
Choose Create function.
On the Code tab, under Code source, paste the following code into the editor pane of the code editor on the lambda_function tab. This code stops the instances that you identify:
```
import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
```

Replace us-west-1 with the AWS Region that your instances are in. Replace InstanceIds with the IDs of the instances that you want to stop and start.
Choose Deploy.
On the Configuration tab, choose General configuration, and then choose Edit.
Set Timeout to 10 seconds, and then choose Save.
Note: (Optional) You can adjust the Lambda function settings. For example, to stop and start multiple instances, you might use a different value for Timeout and Memory.
Repeat steps 1-7 to create another function. Complete the following steps so that this function starts your instances:
In step 3, enter a different Function name. For example, "StartEC2Instances".
In step 5, paste the following code into the editor pane of the code editor on the lambda_function tab:
```
import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances))
          Use your Region and the same instances IDs.
```
Test your Lambda functions
1.Open the Lambda console, and then choose Functions.
2.Choose one of the functions.
3.Choose the Code tab.
4.In the Code source section, choose Test.
5.In the Configure test event dialog box, choose Create new test event.
6.Enter an Event name. Then, choose Create.
7.Note: Don't change the JSON code for the test event.
8.Choose Test to run the function.
9.Repeat steps 1-7 for the other function.
10.Check the status of your instances
11.AWS Management Console

Before and after you test, check the status of your instances to confirm that your functions work.
