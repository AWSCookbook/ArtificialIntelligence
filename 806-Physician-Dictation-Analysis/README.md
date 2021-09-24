# Physician Dictation Analysis
### Set a unique suffix to use for the S3 bucket name:
```
RANDOM_STRING=$(aws secretsmanager get-random-password \
--exclude-punctuation --exclude-uppercase \
--password-length 6 --require-each-included-type \
--output text \
--query RandomPassword)
```

### Create a S3 bucket:

`aws s3api create-bucket --bucket awscookbook806-$RANDOM_STRING`

### Use Amazon Polly to generate a sample primary care physician dictation of patient information and save as dictation.mp3:
```
aws polly synthesize-speech \
--output-format mp3 \
--voice-id Joanna \
--engine neural \
--text 'Patient Jane Doe experiencing symptoms of headache. Administer 200mg Ibuprofen twice daily.' \
dictation.mp3
```

### Upload the mp3 file to your S3 bucket:

`aws s3 cp ./dictation.mp3 s3://awscookbook806-$RANDOM_STRING/`


## Clean up 
### Remove the S3 objects:
```
aws s3 rm \
s3://awscookbook806-$RANDOM_STRING/medical/aws-cookbook-806.json
aws s3 rm \
s3://awscookbook806-$RANDOM_STRING/awscookbook806.json
aws s3 rm \
s3://awscookbook806-$RANDOM_STRING/dictation.mp3
aws s3 rm \
s3://awscookbook806-$RANDOM_STRING/.write_access_check_file.temp
```

### Delete the S3 bucket:

`aws s3api delete-bucket --bucket awscookbook806-$RANDOM_STRING`

### Delete the medical transcription job:
```
aws transcribe delete-medical-transcription-job \
--medical-transcription-job-name aws-cookbook-806
```

