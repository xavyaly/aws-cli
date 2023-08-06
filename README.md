# aws-cli commands

# List all the mfa associated devices
$ aws iam list-virtual-mfa-devices

# Stop an EC2 Instance
$ aws ec2 stop-instances --instance-ids <instance-id>

# Create a Lambda function
$ aws lambda create-function \
    --function-name "MyLambdaFunction" \
    --runtime "nodejs18.x" \
    --role "arn:aws:iam::ACCOUNT_ID:role/lambda-basic-execution-role" \
    --handler "app/index.handler" \
    --timeout 5 \
    --zip-file "fileb://./app.zip" \
    --region "us-east-2"

# Create an API 
$ aws apigateway create-rest-api \
  --name MyAPIGateway

