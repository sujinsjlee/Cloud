# AWS Monitoring
> [CloudTrail](#CloudTrail)  
> [CloudWatch](#CloudWatch)


## CloudTrail
- [CloudTrail](https://aws.amazon.com/ko/cloudtrail/faqs/?nc1=h_ls)
    - Provides governance, compliance and audit for your AWS Account
    - Can put logs from CloudTrail into CloudWatch Logs or **S3**
### Security
- **CloudTrail Log File Encryption using AWS Key Management Service (KMS)**
    - Q: What is the benefit of CloudTrail log file encryption using Server-side Encryption with KMS?
        - CloudTrail log file encryption using SSE-KMS allows you to add an additional layer of **security** to CloudTrail log files delivered to an Amazon S3 bucket by encrypting the log files with a KMS key. By default, CloudTrail will encrypt log files delivered to your Amazon S3 bucket using Amazon S3 server-side encryption.

- **CloudTrail Log File Integrity Validation**
    - Q: What is CloudTrail log file integrity validation?
        - CloudTrail log file integrity validation feature allows you to determine whether a CloudTrail log file was unchanged, deleted, or modified since CloudTrail delivered it to the specified Amazon S3 bucket.
    - Q: What is the benefit of CloudTrail log file integrity validation?
        - You can use the log file integrity validation as an aid in your IT **security** and auditing processes.

## CloudWatch
### CloudWatch Unified Agent – Metrics
- Collected directly on your Linux server / EC2 instance
- CPU (active, guest, idle, system, user, steal)
- Disk metrics (free, used, total), Disk IO (writes, reads, bytes, iops)
- RAM (free, inactive, used, total, cached)
- Netstat (number of TCP and UDP connections, net packets, bytes)
- Processes (total, dead, bloqued, idle, running, sleep)
- Swap Space (free, used, used %)

### Amazon EventBridge
- EventBridge is the next evolution of CloudWatch Events