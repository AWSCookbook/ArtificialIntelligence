# Transcribing a podcast
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

`aws s3api create-bucket --bucket awscookbook801-$RANDOM_STRING`



## Clean up 
### Remove the S3 objects:
``` 
aws s3 rm s3://awscookbook801-$RANDOM_STRING/awscookbook-801.json
aws s3 rm s3://awscookbook801-$RANDOM_STRING/podcast.mp3
aws s3 rm s3://awscookbook801-$RANDOM_STRING/.write_access_check_file.temp
```

### Delete the S3 bucket:

`aws s3api delete-bucket --bucket awscookbook801-$RANDOM_STRING`

### Delete the transcription job:
```
aws transcribe delete-transcription-job \
--transcription-job-name awscookbook-801
```

`unset $RANDOM_STRING`
