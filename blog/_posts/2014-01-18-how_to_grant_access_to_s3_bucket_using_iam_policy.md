---
layout: post
title: "How to Grant Access to S3 Bucket Using IAM Policy"

tags: [aws, s3, iam]

meta-description: "After struggling with writing IAM policy to grant access to a S3 bucket, I decided to share my how-to with other people."
meta-robots: ""
---

This blog is hosted on [Amazon S3][aws-s3] as briefly mentioned in [another post]({% post_url 2013-06-07-blogging_with_jekyll %}).

One thing I overlooked when I was setting it up was security. Inspired by Paul Stamatiou's [blog post](http://paulstamatiou.com/hosting-on-amazon-s3-with-cloudfront/), I decided to create an IAM (Identity and Access Management) credential for my website. Benefit of creating an IAM credential is that in case its access/secret keys get compromised, it only affects the S3 bucket hosting my website.

After wasting my time struggling with creating IAM credential and writing IAM policy to grant access to a S3 bucket, I decided to share my step-by-step guide with other people to save their time.


1. Go to [IAM Console][iam-console].
2. Click on 'Create a New Group of Users'. When the popup dialog comes up:
3. Give it a meaningful group name.
4. Select 'Amazon S3 Full Access' policy template.
5. Inside 'Policy Document' textarea, fill in something like

  ```javascript
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [ "s3:ListBucket" ],
          "Resource": [ "arn:aws:s3:::<bucket-name>" ]
        },
        {
          "Effect": "Allow",
          "Action": [ "s3:*" ],
          "Resource": [ "arn:aws:s3:::<bucket-name>/*" ]
        }
      ]
    }
  ```

6. Click on 'Continue'.
7. Create New Users by specifying new user names or Add Existing Users.
8. Go through the rest of the steps by clicking on 'Continue' buttons.


Now you have a new IAM group and user(s) who belong to that group. Since the group's policy allows `s3:ListBucket` action on your bucket and `s3:*` actions (e.g. GET, DELETE, PUT, etc.) on your bucket's contents, the users who belong to this group can modify S3 bucket to update the website.

When you created the new user, you must have gotten its access/secret key pairs. Now you can use those keys to programmatically upload (e.g. using [jekyll-s3][]) files to your S3 bucket.

Hope this helped you saving some time!

[aws-s3]: http://aws.amazon.com/s3/
[iam-console]: https://console.aws.amazon.com/iam
[jekyll-s3]: https://github.com/laurilehmijoki/jekyll-s3
