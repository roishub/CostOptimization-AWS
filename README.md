# Project Overview: EBS Snapshot Management Lambda Function

This project involves implementing an AWS Lambda function using Python and Boto3 to manage EBS (Elastic Block Store) snapshots efficiently. Here's a detailed yet concise explanation:
## Objective:
The goal is to automate the management of EBS snapshots owned by the AWS account ('self'). This includes identifying and deleting stale snapshots to optimize storage costs while ensuring that snapshots tagged as 'important' are preserved for critical backup purposes.

## Functionality:

1. Snapshot Retrieval: The Lambda function first retrieves all EBS snapshots belonging to the account using the describe_snapshots API call.

2. Active Instance Identification: It then gathers a list of active EC2 instances that are currently running (instance-state-name filter with value 'running') using the describe_instances API call.

3. Snapshot Evaluation: For each snapshot:

  - It checks if the snapshot is tagged with 'important'. Snapshots with this tag are skipped from deletion to ensure critical data backups are retained.
  - If the snapshot is not tagged as 'important' and does not have a volume attached (VolumeId is None), it deletes the snapshot using delete_snapshot API call and logs the action.
  - If the snapshot has a volume attached, it verifies if the volume is currently attached to any running instance. If not, it deletes the snapshot to remove unnecessary backups associated with inactive resources.

4. Error Handling: The function includes error handling to manage situations where volumes associated with snapshots may no longer exist (InvalidVolume.NotFound error). It ensures robustness in snapshot management operations.

## Benefits:

  - Cost Optimization: By automatically removing stale snapshots, the function helps optimize storage costs associated with unused or obsolete backups.
  - Backup Integrity: Snapshots tagged as 'important' are safeguarded, ensuring critical data backups are retained as per organizational policies.

## Usage:

  - This Lambda function is designed to run periodically or in response to specific events (e.g., CloudWatch Events). It integrates seamlessly into CI/CD pipelines or deployment workflows to maintain efficient AWS resource management practices.

## Conclusion:
In summary, this Lambda function plays a crucial role in automating EBS snapshot management within AWS environments. It combines automation with selective retention policies (via tagging) to ensure both cost efficiency and data integrity in backup operations. This approach aligns with modern cloud infrastructure best practices, providing scalable and reliable management of AWS resources.
