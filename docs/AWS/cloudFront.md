# CloudFront
> [CloudFront georestriction](#GeoRestrictions)  
> [Global Accelerator](#Global-Accelerator)  
> [CloudFront Features](#Features)  
> [S3 Cross Region Replication](#S3-Cross-Region-Replication)  
> [origin](#origin)  

## CloudFront
- **Content Delivery Network (CDN)**
- **Improves read performance, content is cached at the edge**
- S3 bucket
  - For distributing files and caching them at the edge
  - Enhanced security with CloudFront Origin Access Identity (OAI)

## GeoRestriction
- [CloudFront georestriction](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html) : *You can use geo restriction, also known as geo blocking, to prevent users in specific geographic locations from accessing content that you're distributing through a CloudFront web distribution.*

## Global Accelerator
- **Q: AWS Global Accelerator와 Amazon CloudFront의 차이점**
  - [FaQ](https://aws.amazon.com/ko/global-accelerator/faqs/)
  - A: Global Accelerator 및 Amazon CloudFront는 AWS 글로벌 네트워크와 전 세계에 분포된 해당 엣지 로케이션을 사용하는 별도의 두 서비스입니다. CloudFront는 캐시 가능한 콘텐츠(예: 이미지, 비디오)와 동적인 콘텐츠(예: API 가속화 및 동적 사이트 제공)의 성능을 모두 개선합니다. Global Accelerator는 엣지에서 패킷을 단일 또는 여러 AWS 리전에서 실행되는 애플리케이션으로 프록시하여 TCP 또는 UDP를 통해 광범위한 애플리케이션의 성능을 개선합니다. Global Accelerator는 게임(UDP), IoT(MQTT) 또는 VoIP와 같은 HTTP 외 사용 사례는 물론, 특별히 정적 IP 주소 또는 결정적 빠른 지역 장애 복구를 요구하는 HTTP 사용 사례에 적합합니다. 두 서비스 모두 DDoS 공격을 막기 위해 AWS Shield와 통합되어 있습니다.

- **AWS Global Accelerator** 
  - Improves performance for a wide range of applications over TCP or UDP
  - Proxying packets at the edge to applications running in one or more AWS Regions.
  - `Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP`
  - Good for HTTP use cases that require static IP addresses
  - Good for HTTP use cases that required deterministic, fast regional failover
- **Amazon CloudFront**
  - Improves performance for both cacheable content (such as images and videos)
  - Content is served at the edge
  - CloudFront is better for improving application resiliency to `handle spikes in traffic`

## Features
- [On-Demand & Real-Time](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/on-demand-streaming-video.html)
  - **You can use CloudFront to deliver video on demand (VOD) or live streaming video using any HTTP origin.** 

## S3 Cross Region Replication
- **CloudFront vs S3 Cross Region Replication**
- CloudFront:
  - Global Edge network
  - Files are cached for a TTL (maybe a day)
  - **Great for static content that must be available everywhere**
- S3 Cross Region Replication:
  - Must be setup for each region you want replication to happen
  - Files are updated in near real-time
  - Read only
  - **Great for dynamic content that needs to be available at low-latency in few regions**

## origin
- CloudFront – Origin Groups
  - To **increase high-availability and do failover**
  - Origin Group: one primary and one secondary origin
  - If the primary origin fails, the second one is used
  