# AWS

## S3

### Mass delete objects

1. List all objects in bucket: `aws s3api list-objects --output text --bucket <BUCKET_NAME> --prefix <PREFIX> --query 'Contents[].[Key]' | pv -l > BUCKET.keys`
2. Delete by batches: `tail -n+0 BUCKET.keys | pv -l | grep -v -e "'" | tr '\n' '\0' | xargs -0 -P1 -n1000 bash -c 'aws s3api delete-objects --bucket <BUCKET> --delete "Objects=[$(printf "{Key=%q}," "$@")],Quiet=true"' _`

Thanks: https://serverfault.com/questions/679989/most-efficient-way-to-batch-delete-s3-files#comment1200074_917740

## IAM

### Assume role and export environment variables

```
export $(printf "AWS_ACCESS_KEY_ID=%s AWS_SECRET_ACCESS_KEY=%s AWS_SESSION_TOKEN=%s" \
$(aws sts assume-role \
--role-arn arn:aws:iam::123456789012:role/MyAssumedRole \
--role-session-name MySessionName \
--query "Credentials.[AccessKeyId,SecretAccessKey,SessionToken]" \
--output text))
```

Thanks: https://stackoverflow.com/a/67636523
