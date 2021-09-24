# Redacting PII from text using Comprehend
## Preparation
### Set a unique suffix to use for the S3 BucketName:
```
RANDOM_STRING=$(aws secretsmanager get-random-password \
--exclude-punctuation --exclude-uppercase \
--password-length 6 --require-each-included-type \
--output text \
--query RandomPassword)
```

### Create a S3 Bucket:

`aws s3api create-bucket --bucket awscookbook804-$RANDOM_STRING`

### Create a path for output in your S3 bucket:
```
aws s3api put-object --bucket awscookbook804-$RANDOM_STRING \
--key redacted_output/
```


## Clean up 
### Detach the AmazonS3FullAccess policy from the role:
```
aws iam detach-role-policy --role-name AWSCookbook804Comprehend \
--policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

### Delete the IAM Role:

`aws iam delete-role --role-name AWSCookbook804Comprehend`

### Delete the objects in your S3 bucket:
```
aws s3 rm s3://awscookbook804-$RANDOM_STRING/sample_data.txt
aws s3 rm s3://awscookbook804-$RANDOM_STRING/redacted_output/.write_access_check_file.temp
aws s3 rm ${S3_LOCATION}sample_data.txt.out
aws s3 rm ${S3_LOCATION}
aws s3 rm s3://awscookbook804-$RANDOM_STRING/redacted_output/
```

### Delete the S3 bucket:

`aws s3api delete-bucket --bucket awscookbook804-$RANDOM_STRING`

### Unset the environment variable that you created manually:
```
unset RANDOM_STRING
unset JOB_ID
unset S3_LOCATION
```

