# aws-cli commands

# START AN INSTANCE

$ aws ec2 start-instances --instance-ids i-0f88a9d21acadac87 --region us-east-2
{
    "StartingInstances": [
        {
            "CurrentState": {
                "Code": 0,
                "Name": "pending"
            },
            "InstanceId": "i-0f88a9d21acadac87",
            "PreviousState": {
                "Code": 80,
                "Name": "stopped"
            }
        }
    ]
}


----------------------------------------------------------------------------------------------------

# STOP AN INSTANCE

$ aws ec2 stop-instances --instance-id i-0f88a9d21acadac87 --region us-east-2
{
    "StoppingInstances": [
        {
            "CurrentState": {
                "Code": 64,
                "Name": "stopping"
            },
            "InstanceId": "i-0f88a9d21acadac87",
            "PreviousState": {
                "Code": 16,
                "Name": "running"
            }
        }
    ]
}

----------------------------------------------------------------------------------------------------

# CREATE A VPC

# LIST OF COMPONENTS CREATED

* Subnets
* Security Groups
* Network ACLs
* Internet Gateways
* Egress Only Internet Gateways
* Route Tables
* Network Interfaces
* Peering Connections
* Endpoints

$ aws ec2 create-vpc --cidr-block 10.0.0.0/16

Output:
{
    "Vpc": {
        "CidrBlock": "10.0.0.0/16",
        "DhcpOptionsId": "dopt-bc3e38d4",
        "State": "pending",
        "VpcId": "vpc-0258b88c832c80e41",
        "InstanceTenancy": "default",
        "Ipv6CidrBlockAssociationSet": [],
        "CidrBlockAssociationSet": [
            {
                "AssociationId": "vpc-cidr-assoc-000f3bef5720793f8",
                "CidrBlock": "10.0.0.0/16",
                "CidrBlockState": {
                    "State": "associated"
                }
            }
        ],
        "IsDefault": false,
        "Tags": []
    }
}

----------------------------------------------------------------------------------------------------

# CREATE A KEY PAIR 

```
https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-keypairs.html
```

$ aws ec2 create-key-pair --key-name WebServer --query 'KeyMaterial' --output text > WebServer.pem
$ cat WebServer.pem           # FOR VERIFICATION
$ chmod 400 WebServer.pem     # PERMISSION
$ aws ec2 describe-key-pairs --key-name WebServer
{
    "KeyPairs": [
        {
            "KeyFingerprint": "1b:1d:35:ae:25:16:24:a6:32:2d:7f:fc:e9:0f:1c:41:4d:ad:94:d5",
            "KeyName": "WebServer"
        }
    ]
}

----------------------------------------------------------------------------------------------------

# CREATE AN INSTANCE

$ aws ec2 run-instances --image-id ami-01b8d0884f38e37b4 --count 1 --instance-type t2.micro --key-name WebServer --security-group-ids sg-94b254f6 --subnet-id subnet-6d4d7305
{
    "Groups": [],
    "Instances": [
        {
            "AmiLaunchIndex": 0,
            "ImageId": "ami-01b8d0884f38e37b4",
            "InstanceId": "i-0a05643e002432a54",
            "InstanceType": "t2.micro",
            "KeyName": "WebServer",
            "LaunchTime": "2020-05-03T12:05:07.000Z",
            "Monitoring": {
                "State": "disabled"
            },
            "Placement": {
                "AvailabilityZone": "ap-south-1a",
                "GroupName": "",
                "Tenancy": "default"
            },
            "PrivateDnsName": "ip-172-31-44-30.ap-south-1.compute.internal",
            "PrivateIpAddress": "172.31.44.30",
            "ProductCodes": [],
            "PublicDnsName": "",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "StateTransitionReason": "",
            "SubnetId": "subnet-6d4d7305",
            "VpcId": "vpc-e518098d",
            "Architecture": "x86_64",
            "BlockDeviceMappings": [],
            "ClientToken": "",
            "EbsOptimized": false,
            "Hypervisor": "xen",
            "NetworkInterfaces": [
                {
                    "Attachment": {
                        "AttachTime": "2020-05-03T12:05:07.000Z",
                        "AttachmentId": "eni-attach-0195850d6ba4ca63d",
                        "DeleteOnTermination": true,
                        "DeviceIndex": 0,
                        "Status": "attaching"
                    },
                    "Description": "",
                    "Groups": [
                        {
                            "GroupName": "default",
                            "GroupId": "sg-94b254f6"
                        }
                    ],
                    "Ipv6Addresses": [],
                    "MacAddress": "02:17:d4:7b:be:3e",
                    "NetworkInterfaceId": "eni-0437d1d5a48a2d740",
                    "OwnerId": "366042179822",
                    "PrivateDnsName": "ip-172-31-44-30.ap-south-1.compute.internal",
                    "PrivateIpAddress": "172.31.44.30",
                    "PrivateIpAddresses": [
                        {
                            "Primary": true,
                            "PrivateDnsName": "ip-172-31-44-30.ap-south-1.compute.internal",
                            "PrivateIpAddress": "172.31.44.30"
                        }
                    ],
                    "SourceDestCheck": true,
                    "Status": "in-use",
                    "SubnetId": "subnet-6d4d7305",
                    "VpcId": "vpc-e518098d"
                }
            ],
            "RootDeviceName": "/dev/sda1",
            "RootDeviceType": "ebs",
            "SecurityGroups": [
                {
                    "GroupName": "default",
                    "GroupId": "sg-94b254f6"
                }
            ],
            "SourceDestCheck": true,
            "StateReason": {
                "Code": "pending",
                "Message": "pending"
            },
            "VirtualizationType": "hvm",
            "CpuOptions": {
                "CoreCount": 1,
                "ThreadsPerCore": 1
            },
            "CapacityReservationSpecification": {
                "CapacityReservationPreference": "open"
            }
        }
    ],
    "OwnerId": "366042179822",
    "ReservationId": "r-0f2901b5a22dca8d0"
}

----------------------------------------------------------------------------------------------------

# ADD A TAG TO AN INSTANCE

```
https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html#launching-instances
```

$ aws ec2 create-tags --resources i-0a05643e002432a54 --tags Key=Name,Value=tomcatServer

----------------------------------------------------------------------------------------------------

# Delete unused/unassigned/unassociated AWS SGs using Bash and AWS CLI ??

#!/bin/bash
# Delete unused Security Groups

read -p 'Region: ' userRegion
for SG in $(aws ec2 describe-security-groups --query 'SecurityGroups[*].GroupId'  --output text --region $userRegion); do
	aws ec2 delete-security-group --group-id $SG --region $userRegion
done

----------------------------------------------------------------------------------------------------

# Describe-instance-status

$ aws ec2 describe-instance-status --instance-id i-03f724096bd013aab
{
    "InstanceStatuses": [
        {
            "AvailabilityZone": "ap-south-1b",
            "InstanceId": "i-03f724096bd013aab",
            "InstanceState": {
                "Code": 16,
                "Name": "running"
            },
            "InstanceStatus": {
                "Details": [
                    {
                        "Name": "reachability",
                        "Status": "passed"
                    }
                ],
                "Status": "ok"
            },
            "SystemStatus": {
                "Details": [
                    {
                        "Name": "reachability",
                        "Status": "passed"
                    }
                ],
                "Status": "ok"
            }
        }
    ]
}

----------------------------------------------------------------------------------------------------

# Instance terminated

$ aws ec2 terminate-instances --instance-ids i-0d9efcb3a9c2712da
{
    "TerminatingInstances": [
        {
            "CurrentState": {
                "Code": 48,
                "Name": "terminated"
            },
            "InstanceId": "i-0d9efcb3a9c2712da",
            "PreviousState": {
                "Code": 80,
                "Name": "stopped"
            }
        }
    ]
}

----------------------------------------------------------------------------------------------------


# List all the mfa associated devices

$ aws iam list-virtual-mfa-devices

----------------------------------------------------------------------------------------------------

# Stop an EC2 Instance

$ aws ec2 stop-instances --instance-ids <instance-id>

----------------------------------------------------------------------------------------------------

# Create a Lambda function

$ aws lambda create-function \
    --function-name "MyLambdaFunction" \
    --runtime "nodejs18.x" \
    --role "arn:aws:iam::ACCOUNT_ID:role/lambda-basic-execution-role" \
    --handler "app/index.handler" \
    --timeout 5 \
    --zip-file "fileb://./app.zip" \
    --region "us-east-2"

----------------------------------------------------------------------------------------------------

# Create an API 

$ aws apigateway create-rest-api \
  --name MyAPIGateway

----------------------------------------------------------------------------------------------------

aws ec2 run-instances \
    --image-id ami- \   
    --instance-type t2.large \
    --region us-west-2 \
    --subnet-id subnet- \  
    --key-name YourKeyPairName \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=MyInstance}]'

----------------------------------------------------------------------------------------------------
