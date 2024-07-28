# s3-upload-github-action

S3 uploader for Github Actions.

You can upload files or directories to any S3 compatible cloud buckets.

## Usage

See the following example.

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
        uses: koraykoska/s3-upload-github-action@master
        env:
          FILE: ./releases/ # can handle a list of files divided by space
          S3_ENDPOINT: 'ams3.digitaloceanspaces.com'
          S3_BUCKET: ${{ secrets.S3_BUCKET }}
          S3_ACCESS_KEY_ID: ${{ secrets.S3_ACCESS_KEY_ID }}
          S3_SECRET_ACCESS_KEY: ${{ secrets.S3_SECRET_ACCESS_KEY }}
```

# Bucket ACL policy

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<bucket name>/*"
        }
    ]
}
```