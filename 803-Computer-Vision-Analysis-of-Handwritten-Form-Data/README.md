# Computer Vision Analysis of Handwritten Form Data
## Preparation
### Set a unique suffix to use for the S3 bucket name:
```
RANDOM_STRING=$(aws secretsmanager get-random-password \
--exclude-punctuation --exclude-uppercase \
--password-length 6 --require-each-included-type \
--output text \
--query RandomPassword)
```

### Create a S3 bucket:

`aws s3api create-bucket --bucket awscookbook803-$RANDOM_STRING`



## Clean up 
### Delete the file you copied to your S3 bucket:

`aws s3 rm s3://awscookbook803-$RANDOM_STRING/registration_form.png`

### Delete the S3 bucket:

`aws s3api delete-bucket --bucket awscookbook803-$RANDOM_STRING`

### Unset the environment variable that you created manually 

`unset RANDOM_STRING`

### Deactivate your Python virtual environment and go to the root of the chapter:

`deactivate && cd ..`

