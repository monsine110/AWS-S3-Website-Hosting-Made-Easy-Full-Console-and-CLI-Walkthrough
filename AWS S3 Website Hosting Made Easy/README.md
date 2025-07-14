
## HOW TO DEPLOY A STATIC WEBSITE TO AN AMASON S3 BUCKET USING AWS MANAGEMENT CONSOLE 

### Prerequisites

- **An AWS account** You need to have an AWS account. If you don’t have one, you can sign up at the [AWS website](https://aws.amazon.com/).
- Your code or use a ready-starter-code - Download to your  local computer and unzip the folder
- AWS region used is eu-north-1
 
 ### What is Amazon S3 Bucket
S3 Buckets are public cloud storage containers for objects stored in Amazon Simple Storage Service (S3). Object storage is a type of data storage architecture that manages and organizes data as discrete units called objects. In this storage model, data is stored as individual objects, each with its unique identifier, metadata, and data content. These objects can be files, images, videos, documents, or any other form of unstructured or semi-structured data.


### Create S3 Bucket
1.  Navigate to the “AWS Management Console” page, click on  “Services”, click on “Storage”, click on “S3"

![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20220556.png)

2. The Amazon S3 dashboard displays. Click “Create bucket”

![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20220954.png)
3. In the General configuration, enter a “Bucket name” and a region of your choice. Note: Bucket names must be globally unique.![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20221139.png)

4. In the Bucket settings for the Block Public Access section, uncheck the “Block all public access”. It will enable public access to the bucket objects via the S3 object URL. Check the acknowledgement button. Click “Create bucket”
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20221230.png)


### Upload files to S3 Bucket

1. The bucket has been created. Click the “Bucket name”
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20222250.png)

2. once the bucket is open to its contents, click the “Upload” button. Click the "Add files" button, and upload the  file content from your local computer to the S3 bucket. Click upload
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20222401.png)

![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20222507.png)
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20222856.png)

### Secure Bucket

1. Click on the “Permissions” tab.


2. The “Bucket Policy” section shows it is empty. Click on the Edit button.

3. Enter the following bucket policy replacing “monsi-bucket1” with the name of your bucket and click “Save”.
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20230447.png)
You will see warnings about making your bucket public, but this step is required for static website hosting

### Configure S3 Bucket
Go to the Properties tab and then scroll down to edit the Static website hosting section.


Click on the “Edit” button to see the Edit static website hosting screen. Now, enable the Static website hosting. The Hosting type should be “Host a Static Website”. For both “Index document” and “Error document”, enter “index.html” and click “Save”.
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-14%20223843.png)




After successfully saving the settings, check the Static website hosting section again under the Properties tab. You must now be able to view the website endpoint as shown below:
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-15%20102301.png)

# Deploying a static website to an Amazon S3 bucket using the AWS Command Line Interface (CLI) involves several steps. Here’s a comprehensive guide to help you through the process.

### Prerequisites

1. **AWS Account**: You need to have an AWS account. If you don’t have one, you can sign up at the [AWS website](https://aws.amazon.com/).

2. **Install AWS CLI**: Make sure the AWS CLI is installed on your machine. You can download and install it from the [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

3. **Configure AWS CLI**: After installation, configure the CLI with your AWS credentials using the following command:
   ```bash
   aws configure
   ```
   You will be prompted for your AWS Access Key ID, Secret Access Key, default region name, and output format.

4. **Static Website Files**: Prepare the files for your static website (e.g., HTML, CSS, JavaScript, images).

### Step-by-Step Guide to Deploy a Static Website to S3

#### Step 1: Create an S3 Bucket

1. **Create a Bucket**: Use the following command to create an S3 bucket. Replace `your-bucket-name` with a globally unique name for your bucket:
   ```bash
   aws s3api create-bucket --bucket your-bucket-name --region your-region --create-bucket-configuration LocationConstraint=your-region
   ```
   For example, if you are using `eu-north-1`, the command will look like:
   ```bash
   aws s3api create-bucket --bucket your-bucket-name --region us-east-1
   ```
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-16%20124243.png)
2. **Enable Static Website Hosting**: Configure your bucket for static website hosting:
   ```bash
   aws s3 website s3://your-bucket-name --index-document index.html --error-document error.html
   ```
   ![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-16%20125003.png)

#### Step 2: Upload Your Website Files

1. **Copy Files to S3**: Use the following command to sync your local website directory to your S3 bucket. Replace `path/to/your/website/*` with the path to your website files:
   ```bash
   aws s3 sync path/to/your/website/ s3://your-bucket-name/
   ```
![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-16%20183508.png)

#### Step 3: To turn on public access
To turn on public access to an Amazon S3 bucket using the AWS Command Line Interface (CLI), you can use the  command. Amazon S3 provides a set of public access settings that can be configured to help block public access to your S3 buckets.
Command to Turn Off Public Access is
 ```bash
 aws s3api put-public-access-block --bucket your-bucket-name --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
 ```
 ![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-16%20172559.png)

#### Step 3: Set Bucket Policy for Public Access

1. **Set Bucket Policy**: Allow public access to the files in your S3 bucket by creating a policy. You can create a file named `policy2.json` , open in vscode using `code .` the following contents:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::your-bucket-name/*"
           }
       ]
   }
   ```
   ![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-16%20170315.png)
   ![alt text](./AWS%20S3%20WEBSITE%20HOSTING%20IMAGE/Screenshot%202025-03-16%20170652.png)

2. **Apply the Policy**: Use the following command to apply the policy:
   ```bash
   aws s3api put-bucket-policy --bucket your-bucket-name --policy file://policy2.json
   ```

#### Step 4: Access Your Website

1. **Get the Website URL**: Your website can be accessed using the following URL format:
   ```
   http://your-bucket-name.s3-website-your-region.amazonaws.com
   ```
   Replace `your-bucket-name` and `your-region` with your respective values.

### Example of the Complete Commands

Here is a summary of all the commands you need to run, assuming your bucket name is `my-static-site` and you're using the `us-west-2` region:

```bash
# Step 1: Create Bucket
aws s3api create-bucket --bucket my-static-site --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2

# Step 2: Enable Static Website Hosting
aws s3 website s3://my-static-site --index-document index.html --error-document error.html

# Step 3: Upload Files
aws s3 sync ./path/to/your/website/ s3://my-static-site/

# Step 4: Set Bucket Policy
# Create `bucket-policy.json` with the appropriate content

aws s3api put-bucket-policy --bucket my-static-site --policy file://bucket-policy.json

# Access the site
# http://my-static-site.s3-website-us-west-2.amazonaws.com
```






