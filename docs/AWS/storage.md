### 3 types of AWS Storage Gateway:
  - **File Gateway**
    - Configured S3 buckets are accessible using the NFS and SMB protocol
    -  NFS (network file system)
    - Integrated with **Active Directory (AD)** for user authentication
  - **Volume Gateway**
    - Cached volumes: low latency access to most recent data
    - Stored volumes: entire dataset is on premise, scheduled backups to S3
  - **Tape Gateway** : Some companies have backup processes using physical tapes

### AWS Storage Gateway
- Before you create an SMB file share, make sure that you configure SMB security settings for your file gateway. You also configure either Microsoft `Active Directory (AD)` or guest access for authentication.

- **AWS Storage Gateway file gateway** : Configured S3 buckets are accessible using the NFS and SMB protocol
- Amazon FSx for Windows File Server : support SMB, but scenario should contain Windows or AD

### SMB
- **Server Message Block** 
  - SMB는 네트워크 상 존재하는 노드들 간에 자원을 공유할 수 있도록 설계된 프로토콜
- SMB —> storage gateway or FSx

### FsX
- [FsX](https://aws.amazon.com/fsx/windows/)
  - Fully managed file storage built on Windows Server

### AWS Storage Gateway Summary
> Exam tip: Read the question well, it will hint at which gateway to use


- **On-premises data to the cloud** => **Storage Gateway**
- **File access / NFS – user auth with Active Directory** => **File Gateway**(backed by S3)
- **Volumes / Block Storage / iSCSI** => **Volume gateway**(backed by S3 with EBS snapshots)
- **VTL Tape solution / Backup with iSCSI** = > **Tape Gateway**(backed by S3 and Glacier)
- No on-premises virtualization => Hardware Appliance
