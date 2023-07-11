# Hosting a Simple Static Website with AWS EC2 and S3 Bucket

This guide will walk you through the steps to host a simple static website using AWS EC2 and an S3 bucket. AWS EC2 will be used to run a web server, and the S3 bucket will store the website files.

## Prerequisites

Before you begin, make sure you have the following:

- An AWS account
- AWS CLI installed and configured with your AWS account credentials
- Website files ready to be uploaded (HTML, CSS, JS, images, etc.)

## Steps

### 1. Create an S3 bucket

1. Open the [AWS Management Console](https://console.aws.amazon.com/s3/).
2. Click "Create bucket".
3. Enter a unique bucket name and choose the region.
4. Leave all other options as default and click "Create bucket".

### 2. Upload website files to the S3 bucket

1. Open the AWS Management Console and navigate to your newly created bucket.
2. Click "Upload".
3. Select the website files from your local machine and click "Next".
4. Leave all other options as default and click "Upload".

### 3. Create an IAM role with AmazonS3FullAccess policy

1. Open the [IAM Management Console](https://console.aws.amazon.com/iam/).
2. Click "Roles" in the sidebar.
3. Click "Create role".
4. Select the service that will use this role (EC2) and click "Next".
5. In the "Permissions" step, search for and select the "AmazonS3FullAccess" policy.
6. Click "Next" and then provide a name and optional description for the role.
7. Click "Create role".

### 4. Launch an EC2 instance

1. Open the AWS Management Console and navigate to the EC2 service.
2. Click "Launch Instance".
3. Choose an Amazon Machine Image (AMI) of your choice.
4. Select an instance type and click "Next".
5. Configure the instance details as per your requirements and click "Next".
6. Add storage if needed and click "Next".
7. Configure security groups to allow inbound traffic on port 80 (HTTP) and 443 (HTTPS).
8. Review the instance details and click "Launch".
9. Select or create a key pair and click "Launch Instances".

### 5. Attach the IAM role to the EC2 instance

1. Open the AWS Management Console and navigate to the EC2 service.
2. Click "Instances" in the sidebar and select the running instance.
3. Click "Actions" and choose "Instance Settings", then "Attach/Replace IAM Role".
4. Select the IAM role you created in the previous step and click "Apply".

### 6. SSH into the EC2 instance

1. Connect to the EC2 instance using an SSH client like PuTTY or OpenSSH.
2. Run the following command to update the server packages:
    ```
    sudo yum update -y
    ```
3. Install the Apache web server with the following command:
    ```
    sudo yum install httpd -y
    ```
4. Change the directory to the web server's document root:
    ```
    cd /var/www/html
    ```
5. Sync the website files from the S3 bucket to the local directory:
    ```
    aws s3 sync s3://remote_S3_bucket local_directory
    
    #in place of the "remote_S3_bucket" enter the unique buket name you created, and Enter the "local_directory" as /var/www/html 
    ```

### 7. Start the web server and test the website

1. Start the Apache web server with the following command:
    ```
    sudo service httpd start
    ```
2. Open a web browser and enter the public IP address of your EC2 instance.
3. If You are using a chrome Browser make sure to remove the "s" from https:// or else add http:// before the public IP address
4. You should see your static website displayed in the browser.

## Conclusion

Congratulations! You have successfully hosted a simple static website using AWS EC2 and an S3 bucket. The IAM role you created and attached to the EC2 instance ensures that the instance has the necessary permissions to access the S3 bucket.

Remember to clean up and terminate the EC2 instance and S3 bucket when you no longer need them to avoid unnecessary costs.

For more information and advanced configurations, refer to the official AWS documentation.

Note: The files used in hosting are from the public CodePen of LegoMushroom (https://codepen.io/sol0mka/pen/eYgydO)
Github Repo: https://github.com/julianshapiro/velocity


Happy hosting!
