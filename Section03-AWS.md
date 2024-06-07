# AWS

- [AWS](#aws)
  - [Regions](#regions)
  - [Availability Zones](#availability-zones)
  - [Edge Location](#edge-location)
  - [Console](#console)

## Regions

a regions is a cluster of data centers.
Most AWS Services are region-scoped.
If you use a service in a specific region and then change to another region,
it is the same as using the service for the first time.

> Q: if you need to launch a new application, where should you do it?
> A: it depends

factors that affects to choose region
- Compliance with data governance and legal requriements
- Proximity to customers
- Available services within a Region
- Pricing

## Availability Zones

Each region has min of 3 (max of 6) availability zones
(e.g. ap-southeast-2a,ap-southeast-2b, ap-southeast-2c)

Each AZ is one or more discrete data centers with redundant power, networking, and connectivity.
They are separated from each other for isolation from disasters

They are connected with high bandwidth, ultra-low latency neworking.

## Edge Location

AWS has 400+ Points of Presence(400+ Edge Location & 10+ Regional Caches) in 90+ cities across 40+ countries

It makes contents delivered to end users with lower latency.

## Console

- Global Services:
    - IAM : Identity and Access Management
    - Route 53 : DNS service
    - CloudFront : Content Delivery Network
    - WAF : Web Application Firewall
- Region-scoped Services:
    - Amazon EC2 : Infrastructure as a Service
    - Elastic Beanstalk : Platform as a Service
    - Lambda : Function as a Service
    - Rekognition : Software as a Service
