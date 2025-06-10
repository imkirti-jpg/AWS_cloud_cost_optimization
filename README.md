# AWS_cloud_cost_optimization

# AWS Cloud Cost Optimization – Automated Cleanup with Lambda

## Overview
This project automates AWS cloud cost optimization by cleaning up two commonly forgotten resources that incur unnecessary charges:

1. **EBS Stale Snapshots** – Snapshots no longer associated with active EC2 volumes or instances.
2. **Stale S3 Objects** – Unused or old objects in specific S3 buckets based on defined lifecycle rules (e.g., age, tags, or naming patterns).

Using AWS Lambda, the project ensures ongoing cleanup of these stale resources, helping reduce storage costs without manual intervention.

## Features
- **EBS Snapshot Cleaner**:
  - Deletes snapshots not linked to a volume or to volumes not used by running EC2 instances.
  - Avoids deleting snapshots used by active instances.
  - Handles edge cases like deleted volumes.

- **S3 Object Cleaner**:
  - Deletes S3 objects older than a specified number of days.
  - Targets specific buckets or keys using prefix/tag filters.
  - Customizable retention period.

## Prerequisites

### AWS Account
- Ensure access to an AWS account with required permissions.

### IAM Role for Lambda
Attach the following managed policies to the Lambda execution role:
- `describe_snapshots`
- `describe_instances`
- `delete_snapshot`
- `CloudWatchLogsFullAccess`
- `list_buckets`
- `list_objects_v2`
- `delete_bucket`

### Python Environment
- Python 3.7 or higher
- Boto3 AWS SDK
- datetime

Install boto3 with:
- pip install boto3


## Lambda Function Logic

### 1. EBS Snapshot Cleanup (`ebs_cleanup.py`)

**Step 1**: Retrieve all EBS snapshots owned by the account.  
**Step 2**: Fetch IDs of all running EC2 instances and their attached volumes.  
**Step 3**: Delete snapshots not linked to any active volume or EC2 instance.

### 2. S3 Stale Object Cleanup (`s3_cleanup.py`)

**Step 1**: List objects in a target bucket.  
**Step 2**: Check the last modified date.  
**Step 3**: Delete objects older than the defined retention period.

### 3. Test the Functions

- Use the **AWS Lambda Console** to trigger test events for both Lambda functions (`ebs_cleanup.py` and `s3_cleanup.py`).
- Check the **CloudWatch Logs** to verify the resources that were deleted and confirm the function executed successfully.

### Notes
- Always test in a non-production AWS account.
- Use AWS Cost Explorer to monitor impact after deployment.
- Customize retention rules and filters as per your organization’s data policy.

### Future Enhancements
- Add email/SNS notifications after each cleanup.
- Generate detailed CSV reports of deleted resources.
- Add exclusion support for specific S3 prefixes or snapshot tags.

