# aws-cript-thumnails-cretor

The goal of the script is to automate the resizing of images uploaded to a website, maintaining a specific size to ensure proper display in the website section. The script will be developed to work within the Amazon storage service, S3, allowing for automatic resizing without the need for manual intervention.

![Untitled_Artwork](https://user-images.githubusercontent.com/123271041/235210885-e8406a82-cc1f-4465-98fa-fcd1f2579019.jpeg)

First we need to create 2 S3 buckets, one for the original files that will trigger the lambda function and the second to store the thumbnails.

![Screen Shot 2023-04-28 at 12 59 51 PM](https://user-images.githubusercontent.com/123271041/235208816-c63ad3c6-a800-4f5d-a4c8-b3a51fc0e7f7.png)


With the buckets created we can also create the lambda function from scratch, however the lambda doesn’t come with the proper permissions once it’s created, we need to change the permissions on the JSON code on the IAM section. 


![Screen Shot 2023-04-27 at 3 10 50 PM](https://user-images.githubusercontent.com/123271041/235211050-7e59dabd-5867-45f9-a119-2f6aefcc28c2.png)


The policy has three statements, each with its own Effect, Action, and Resource fields:
The first statement allows the IAM role to create and manage CloudWatch logs.
The second statement allows the IAM role to read objects from an S3 bucket.
The third statement allows the IAM role to write objects to the same S3 bucket.
In simpler terms, this policy defines what an IAM role is allowed to do with logs and files in an AWS account. Specifically, it allows the role to create and manage CloudWatch logs, read files from an S3 bucket, and write files to the same S3 bucket.

![Screen Shot 2023-04-27 at 3 39 10 PM](https://user-images.githubusercontent.com/123271041/235211118-38e83421-f901-4014-85cd-00d22ccb4a97.png)

Now the Lambda function can read and write on S3 buckets and we gotta create a trigger to every time a new image be recorded on S3, it will run the Lambda Script:

![Screen Shot 2023-04-27 at 3 48 16 PM](https://user-images.githubusercontent.com/123271041/235211241-13323874-47e6-4f14-aaf3-08f62ebc39da.png)




By selecting a trigger to S3 I select the origin bucket where the images will be stored and select which event will trigger it, in this case each object created on this bucket will run the code. 

![Screen Shot 2023-04-27 at 3 53 00 PM](https://user-images.githubusercontent.com/123271041/235211478-796f8d7e-9382-4714-948b-6fdd7f7fd9d1.png)

As an optional mode you can select the extensions such as .jpg and .png but I will leave as it is. 



The trigger is setup now is time to run the code on the Lambda function, by doing this we face a common problem which is lambda not recognize all python libraries available, we need to use the boto3 and PIL to run this function, and for now we have to upload the library archive and make Lambda able to read an run the code.

![Screen Shot 2023-04-27 at 4 09 14 PM](https://user-images.githubusercontent.com/123271041/235211640-2c6f3a3e-f45e-451c-ba0e-a23b27f9d679.png)

As a best practice I decided to not run the AWS bucket directory directly on the script since it would make it harder in the future to replicate this same lambda function to another instance or in case the bucket be deleted or renamed it would bug the script.  As a best practice from AWS we should use the Environment variables tab to designate our path as described below:


![Screen Shot 2023-04-27 at 4 16 11 PM](https://user-images.githubusercontent.com/123271041/235211372-65a950c7-a783-4971-97e4-3ebe4c35e48b.png)

With the lambda, s3 and permissions setup the code will run and resize original pictures to thumbnails of 128x128 pixels.


