```markdown
# AWS Regions

- AWS Regions are separate geographic areas that contain multiple, isolated locations known as Availability Zones (AZs).
- Regions have codes such as us-east-1, eu-west-1, ap-south-1, etc.
- Most AWS services are region-scoped: resources you create in one region are not automatically available in another region.
- Choosing a region affects latency, cost, compliance (data residency), and available services/features.
- Region partitions: AWS has different partitions such as aws (public), aws-cn (China), and aws-us-gov (GovCloud) with different isolation and compliance properties.

Important exam points about regions:
- Service availability varies by region; new services often launch in a subset of regions first.
- Some services are global or have global components. Examples:
  - Global: IAM, AWS Organizations, Amazon Route 53 (management), AWS WAF (global mode), Amazon CloudFront (edge distribution management).
  - Edge/Global network: CloudFront uses edge locations and Regional Edge Caches.
  - Data services like S3 have a global namespace for bucket names, but buckets are region-specific for data storage (choose region for residency/latency).
- Cross-region replication and architectures:
  - S3 Cross-Region Replication (CRR), RDS cross-region read replicas, and cross-region snapshots exist to copy data between regions.
  - Inter-region data transfer typically incurs additional charges (data egress/ingress pricing). Design for cost and latency.
- Multi-region architectures are used for disaster recovery scenarios (pilot light, warm standby, multi-site active-active) and for global low-latency user access.

## AWS Availability Zones (AZ)

- An Availability Zone is one or more discrete data centers, each with independent power, cooling, and networking.
- A region typically has between 2 and 6 AZs; 3 AZs is common for high-availability patterns.
- AZs within a region are physically separate (different fault domains) but connected with low-latency, high-bandwidth networking to support synchronous replication when supported by services.
- AZ failure is unlikely but possible; architect for AZ failure using multi-AZ deployments. Multi-AZ provides high availability within a region, but not protection against a full region outage.
- AZ identifiers (e.g., us-east-1a, us-east-1b) are mapped differently per AWS account — the same letter does not guarantee the same physical AZ across accounts. Do not rely on fixed mapping across accounts.
- Subnets are created in a VPC per AZ; for fault tolerance, create subnets in multiple AZs and distribute resources (instances, RDS, etc.) across them.
- Many managed AWS services offer built-in multi-AZ options (e.g., RDS Multi-AZ for failover, ElastiCache Multi-AZ, EFS across AZs).

Exam-relevant AZ details:
- Use multi-AZ for high availability (automatic failover) and multi-region for disaster recovery (region-wide failure).
- Some replication can be synchronous between AZs (low-latency links) and asynchronous across regions.
- VPC peering, Transit Gateway, and inter-region peering allow connectivity between VPCs in different regions, but there are costs and latency considerations.

## Edge locations, Local Zones, and Wavelength

- Edge locations: Part of Amazon CloudFront's global network used for caching content closer to users (global edge network).
- Regional edge caches: Intermediate caching layer between origin and edge locations for better hit ratios and performance.
- AWS Local Zones: Extend AWS infrastructure to cities to provide single-digit millisecond latency for latency-sensitive applications.
- AWS Wavelength: Brings compute and storage to the edge within 5G network providers for ultra-low-latency use cases.

## Other important points

- Data residency & compliance: Some customers must keep data in certain geographic regions; choose regions accordingly and be aware of partition-specific constraints (e.g., China and GovCloud regions require separate accounts/approvals).
- Cost and latency trade-offs: Regions can have different pricing for the same services. Placing resources closer to users reduces latency but may increase operational complexity.
- Regional endpoints: Many AWS APIs are region-specific; ensure your application targets the correct regional endpoints.
- API throttling, service limits, and quotas are region-specific in many cases. Some quotas can be increased by request.
- Always design for failure: Use AZ distribution for availability, consider cross-region replication and backups for disaster recovery, and plan for costs and operational overhead of multi-region deployments.

References / practical tips for the exam:
- Know the difference between multi-AZ (high availability within a region) and multi-region (disaster recovery / global footprint).
- Remember per-account AZ name mapping; don't assume "us-east-1a" is the same physical AZ in two accounts.
- Be aware of which services are global vs regional when answering architecture questions (IAM is global; S3 data is regional but bucket names are global).
```
