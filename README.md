# S3 Upload multiple files or directories - Github Action

Github Action S3 Uploader of multiple files or directories.

***Note:*** Kudos to the action creator [koraykoska](https://github.com/koraykoska/s3-upload-github-action) and to the enhancment contributed by [ZabasJC](https://github.com/ZabasJC/s3-upload-github-action).  
(I added minor modifications).


## Prerequisite
While it is not align with best practice of role based access to AWS resources, the current version of this action use Access Key and Secret Access Key in order to grant upload access to the bucket.  
IAM user with PutObject permission to the S3 bucket will need to be created ahead of time. IAM policy can be found in the [IAM Policy](#iam-policy) section below.
Bucket name, as well the access-key and secret-access-key she be stored in secured location.
The example in the [Usage](#usage) below show that these values stored is the repository setting secrets section, however it can be stored in any external vault/secret-manager that the Github runner has access to.


## Usage
- FILE - single file/directory name or list of files/directories , divided by space.
- S3_ENDPOINT - Endpoint url can be found [here](https://docs.aws.amazon.com/general/latest/gr/s3.html) based on the region of the bucket.
- S3_BUCKET - The bucket name.
- S3_ACCESS_KEY_ID - The access key of the IAM account.
- S3_SECRET_ACCESS_KEY - The secret access key of the IAM account.
- (Optional) S3_PREFIX - Prefix to the location of the file(s) or directories.

See the following example for basic usage:

```YAML
# inside .github/workflows/action.yml
name: Add File to Bucket
on: push

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@master

      - name: Upload file to bucket
        uses: ranser102/s3-upload-github-action@main
        env:
          FILE: ./releases/ # can handle a list of files divided by space
          S3_ENDPOINT: 's3.us-east-1.amazonaws.com'
          S3_BUCKET: ${{ secrets.S3_BUCKET }}
          S3_ACCESS_KEY_ID: ${{ secrets.S3_ACCESS_KEY_ID }}
          S3_SECRET_ACCESS_KEY: ${{ secrets.S3_SECRET_ACCESS_KEY }}
```

## IAM Policy

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket name>/*"
            ]
        }
    ]
}
```
