# AWS Networking Assignment

This repository documents the creation of a VPC with subnets across two Availability Zones, a NAT Gateway for private subnets, and security group/NACL configurations. All resources were created using the AWS Management Console and tested with a bastion host.

## 1. VPC with Subnets

- **VPC CIDR**: 10.0.0.0/16
- **Subnets**:
  - **Public** (AZ1): 10.0.0.0/24
  - **Public** (AZ2): 10.0.1.0/24
  - **Private** (AZ1): 10.0.2.0/24
  - **Private** (AZ2): 10.0.3.0/24


## 2. NAT Gateway Setup and Testing

### Setup
- Created two Elastic IPs.
- Deployed NAT Gateways in each public subnet (`nat-gateway-1a` and `nat-gateway-1b`).
- Updated the private route tables to route `0.0.0.0/0` to the respective NAT Gateway.


### Testing the NAT Gateway
I used a session manager to connect to a private instance and verify internet access.

**Steps**:
1. Inside the private instance, ran `curl ifconfig.me`.

The output returned the public IP of the NAT Gateway, confirming outbound internet access.


## 3. Security Group and Network ACL

### Security Group (`web-sg`)
- **Inbound**: SSH (22), HTTP (80), HTTPS (443) from 0.0.0.0/0.
- **Outbound**: All traffic allowed.


### Network ACL (`private-nacl`)
- **Stateless**: Inbound and outbound rules are separate.
- **Rules**: Allowed all traffic for demonstration (rules 100 for both directions).
- **Associated with private subnets**.

### JSON Configuration
- [nacl-inbound.json](config/nacl-inbound.json)
- [nacl-outbound.json](config/nacl-outbound.json)
