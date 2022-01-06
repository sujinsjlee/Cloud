# AWS EC2

## EC2 Nitro
- Allows for better performance:
    - Better networking options (enhanced networking, HPC, IPv6)
    - **Higher Speed EBS (Nitro is necessary for 64,000 EBS IOPS** – max 32,000 on non-Nitro)
- Underlying Platform for the next generation of EC2 instances
- New virtualization technology


## EC2 Instance Store
- EBS volumes are network drives with good but “limited” performance
- If you need a high-performance hardware disk, use EC2 Instance Store

- Better I/O performance
- EC2 Instance Store lose their storage **if they’re stopped (ephemeral)** --> temporary 일시적인 저장
- Good for buffer / cache / scratch data / temporary content
- Risk of data loss if hardware fails
- Backups and Replication are your responsibility