---
title: "Blog 1"
date: 2026-07-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Accessing Private Amazon S3 from a VPC Using a VPC Endpoint

_This article is shared by the Otokoshi team based on our learning and hands-on practice with AWS._

When first deploying demo models on AWS, the simplest and easiest-to-understand approach for an EC2 Instance to upload or download data with Amazon S3 is to place the EC2 instance in a public subnet, assign it a public IP address, configure a route through an Internet Gateway, and then use the AWS CLI or SDK to interact with S3.
This approach is suitable for learning environments or quick demos because the configuration is not too complex. However, in production systems such as backend services, internal services, or data processing systems, resources are usually placed inside a **private subnet** to limit direct access from the Internet. In that case, allowing data to travel through the public internet is no longer an optimal choice, especially when the system requires strong security, data control, and operational cost optimization.
Therefore, this article explains how to design a private access model to Amazon S3 from inside a VPC using a **Gateway VPC Endpoint**. This solution allows EC2 instances in a private subnet to communicate with S3 without using an Internet Gateway or NAT Gateway.

## Background and Problem Statement

Assume that the system has an internal backend running on an EC2 Instance inside a private subnet. This EC2 instance does not have a public IP address and is not exposed directly to the Internet in order to protect the system. However, the backend still needs to connect to Amazon S3 to perform several important tasks, such as:
- Storing system log files.
- Exporting periodic reports.
- Backing up configuration files and batch data to Amazon S3.
When a private subnet has no route to the Internet, the EC2 instance cannot directly access the default S3 endpoint. A common solution is to configure a NAT Gateway so that resources in the private subnet can access the Internet. However, NAT Gateway may generate costs based on hourly usage and data processing volume.
Instead of using a NAT Gateway only to access S3, we can deploy a **Gateway VPC Endpoint for Amazon S3**. This solution creates a private connection between the VPC and Amazon S3, allowing traffic to avoid the public Internet while improving security and cost efficiency.

## Overall Architecture and Processing Flow

The MVP (Minimum Viable Product) model in this article includes the following core components:
- **EC2 Instance**: Placed inside a private subnet and does not have a public IP address.
- **Amazon S3 Bucket**: Acts as the target data storage and has _Block Public Access_ enabled.
- **Gateway VPC Endpoint**: A Gateway-type VPC endpoint used to privately connect to Amazon S3.
- **Route Table**: The route table of the private subnet is configured to direct traffic to S3 through the endpoint.
- **IAM & Policies**: Includes the IAM Role for EC2, Endpoint Policy, and Bucket Policy to control access permissions across multiple layers.

### Data Processing Flow

The process can be understood through the following steps:
1. The EC2 Instance in the private subnet sends a read or write request to Amazon S3.
2. The route table of the private subnet identifies traffic whose destination is S3 through the AWS Prefix List.
3. The traffic is routed through the **Gateway VPC Endpoint** instead of going through an Internet Gateway or NAT Gateway.
4. Amazon S3 receives the request and checks access permissions through multiple security layers, including the EC2 IAM Role, Endpoint Policy, and Bucket Policy.
5. If all security conditions are valid, the EC2 instance can interact with the S3 Bucket privately and securely.
With this architecture, the backend inside the private subnet can still use Amazon S3 for storage tasks without opening a connection to the public Internet.

## Solution Choice: VPC Endpoint vs NAT Gateway

VPC Endpoint and NAT Gateway both play important roles in AWS network architecture, but they serve different purposes. A VPC Endpoint does not completely replace a NAT Gateway. It is more suitable when resources inside a VPC need private connectivity to certain supported AWS services, such as Amazon S3 or DynamoDB.
The table below shows the basic differences between a Gateway VPC Endpoint and a NAT Gateway:
| Criteria | Gateway VPC Endpoint (S3/DynamoDB) | NAT Gateway |
| ------------------------ | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Connection scope** | Provides private connectivity only to supported AWS services such as S3 and DynamoDB. | Allows resources in a private subnet to access the Internet, such as calling external APIs or updating packages. |
| **Traffic path** | Traffic stays within the AWS internal network through the AWS backbone network. | Traffic goes through the public Internet. |
| **Cost** | No hourly charge and no data processing charge for Gateway Endpoints. | Charged based on hourly usage and data processing per GB. |
| **Network configuration** | Updates the Route Table with the Endpoint ID as the target (`vpce-xxx`). | Updates the Route Table with the NAT Gateway ID as the target (`nat-xxx`). |
From the table above, it can be seen that if the main requirement is only to allow EC2 in a private subnet to access Amazon S3, a Gateway VPC Endpoint is the more suitable choice. This solution reduces dependency on the public Internet while avoiding unnecessary costs from NAT Gateway.

## Important Security Considerations

> **Architecture tip:** A VPC Endpoint mainly solves the traffic path problem at the Network layer. It does not automatically grant data access permissions to EC2. Therefore, for the system to operate securely, multiple layers of access control should be combined using the **Defense in Depth** approach.
In this model, there are 3 important security layers that need to be configured carefully:
- **EC2 IAM Role**: Defines which actions the EC2 Instance is allowed to perform on Amazon S3, such as `s3:GetObject` or `s3:PutObject`.
- **Endpoint Policy**: Limits which actions or buckets are allowed to pass traffic through the VPC Endpoint.
- **Bucket Policy**: Controls access requests to the bucket. To strengthen security, the Bucket Policy can be configured to accept requests only from a specific VPC Endpoint through the `aws:sourceVpce` condition.
Example of a Bucket Policy that only allows access through a specific VPC Endpoint:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Allow-Access-From-Specific-VPCE-Only",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "aws:sourceVpce": "vpce-1a2b3c4d"
        }
      }
    }
  ]
}
```
This configuration ensures that access requests to the bucket are valid only when they go through the specified VPC Endpoint. If a request comes from another source, even if it has certain access permissions, the system can still deny it based on the configured security policy.

## Debugging Experience When Connection Errors Occur

During implementation, if EC2 cannot connect to Amazon S3 or access is denied, we should not only focus on checking the source code. The issue may come from many different layers in the architecture, including IAM, Route Table, DNS, Region, or Policy.
A basic debugging checklist may include:
- [ ] **IAM Role**: Has the correct IAM Role been attached to the EC2 instance? Does this role have the minimum required permissions based on the Least Privilege principle?
- [ ] **Route Table**: Does the private subnet have a route that directs traffic to S3 through `vpce-xxx`?
- [ ] **DNS & Region**: Are the bucket name and region in the code or AWS CLI configured correctly?
- [ ] **Endpoint Policy**: Is the Endpoint Policy incorrectly restricting the bucket or action?
- [ ] **Bucket Policy**: Is there any `Deny` condition in the Bucket Policy that accidentally blocks the request?
- [ ] **Block Public Access**: Has Block Public Access been enabled on the bucket, and is the access policy suitable for the private access model?
Checking each layer as above makes the troubleshooting process clearer. Instead of guessing that the error is in the code, the implementer can accurately identify whether the request is being blocked at the network layer, IAM layer, or policy layer.

## Lessons Learned

Through the process of practicing the implementation of private Amazon S3 access from a VPC, the team learned several important lessons:
1. **A private subnet does not mean that resources are completely isolated**
  Resources inside a private subnet are not directly accessible from the Internet, but they can still communicate with necessary AWS services if the network architecture is designed properly. Gateway VPC Endpoint allows EC2 instances in a private subnet to access Amazon S3 privately, securely, and without requiring a public IP address.
2. **Security should be integrated from the architecture design stage**
  VPC Endpoint is not only a network configuration. When combined with IAM Role, Endpoint Policy, and Bucket Policy, it helps reduce the attack surface by removing the need to open public Internet connectivity when it is not necessary. This is a practical example of multi-layered security thinking.
3. **A good Cloud system requires coordination between Network and Security**
  Cloud architecture is not only about creating resources. A well-designed system needs to clearly understand where traffic flows, how route tables direct traffic, which layer permissions are granted at, and how the bucket controls access. The combination of Network components such as Route Table and VPC Endpoint with Security components such as IAM, Bucket Policy, and Logging is the foundation for operating a secure and optimized system.

## Conclusion

Gateway VPC Endpoint is a suitable solution when resources in a private subnet need to access Amazon S3 privately. Instead of allowing EC2 to access the Internet through a NAT Gateway, this model helps route S3 traffic within the AWS internal network, reducing security risks and optimizing operational costs.
In practice, when designing a cloud system, the choice between NAT Gateway and VPC Endpoint should depend on the specific connectivity requirements. If the system needs to access many external Internet services, NAT Gateway is still a necessary component. However, if the goal is only to access supported AWS services such as Amazon S3, VPC Endpoint is a simpler, safer, and more efficient choice.
More importantly, the biggest lesson from this model is: cloud security does not only come from placing resources inside a private subnet, but also from how the traffic path is designed, how IAM permissions are assigned, and how data is controlled at each layer of the system.

![Illustration](/images/3-BlogsTranslated/Blog1/blog1.jpg)