# 🚀 3-Tier Architecture on AWS (High Availability & Secure)

A secure, scalable, and highly available **3-Tier Web Architecture** deployed manually on AWS, featuring public/private subnet isolation, an Application Load Balancer, private application servers running Apache and phpMyAdmin, and a Multi-AZ MySQL database.

---

## 🏗️ Architecture Overview


                        INTERNET
                            |
                    [ Route 53 / Browser ]
                            |
                ┌───────────────────────┐
                │   ALB (my-alb)        │  ← Tier 1 (Presentation)
                │   internet-facing     │
                │   ap-south-1a/b/c     │
                └───────────────────────┘
                        |         |
            ┌───────────┐         ┌───────────┐
            │app-server-1│         │app-server-2│  ← Tier 2 (Application)
            │10.0.4.235  │         │10.0.5.22   │
            │Apache +    │         │Apache +    │
            │phpMyAdmin  │         │phpMyAdmin  │
            └───────────┘         └───────────┘
                        |         |
                ┌───────────────────────┐
                │   RDS (my-db)         │  ← Tier 3 (Database)
                │   MySQL 8.4.9         │
                │   Multi-AZ enabled    │
                │   Privately accessible│
                └───────────────────────┘
🛠️ Components & Implementation Steps
1. Networking & VPC (Tier Foundation)

✔ Custom VPC: Configured a custom Virtual Private Cloud spanning 3 Availability Zones (ap-south-1a, ap-south-1b, ap-south-1c).

✔ Subnets: Set up public subnets for the Internet-Facing Application Load Balancer and private subnets for application servers and databases.

✔ NAT Gateway: Provisioned a NAT Gateway to allow private application instances to securely fetch updates from the internet without exposing them inbound.

2. Tier 1: Presentation Layer

✔ Application Load Balancer (my-alb): Deployed an internet-facing ALB across 3 AZs to accept HTTP traffic on port 80.

✔ Target Groups (my-alb-app-tg): Configured target groups routing traffic to backend EC2 instances with active health checks verifying a 100% healthy status.

3. Tier 2: Application Layer

✔ EC2 Instances: Launched two application servers (app-server-1 at 10.0.4.235 and app-server-2 at 10.0.5.22) residing entirely inside secure private subnets.

✔ Software Stack: Configured both servers with Apache (2.4.68) and phpMyAdmin running on Amazon Linux.

4. Tier 3: Database Layer

✔ Amazon RDS (my-db): Deployed MySQL 8.4.9 inside a private subnet.

✔ High Availability: Enabled Multi-AZ replication for automated failover and fault tolerance.

✔ Security Isolation: Ensured the database is completely private and only reachable via port 3306 from the application security groups.

🔒 Security Highlights

✔ Private Subnets: Compute and database tiers are completely hidden from direct public internet access.

✔ Layered Security Groups: Enforced strict traffic rules: Internet -> ALB -> EC2 (Apache) -> RDS (MySQL).

✔ Single Entry Point: All inbound user traffic flows exclusively through the secure Application Load Balancer.

📊 Verification & Proof

✔ End-to-end connectivity was successfully verified by accessing the phpMyAdmin dashboard via the public ALB URL, confirming seamless communication through the private Apache servers down to the Multi-AZ MySQL database.

🚀 AWS Services Used

✔ Amazon VPC (Subnets, NAT Gateway, Route Tables)

✔ Amazon EC2 & Application Load Balancer (ALB)

✔ Amazon RDS (MySQL 8.4.9 with Multi-AZ)

✔ IAM & Security Groups
