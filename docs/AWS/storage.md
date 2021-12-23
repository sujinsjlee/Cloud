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