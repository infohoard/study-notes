# AWS Solutions Architect Associate Study Notes

## Elastic Compute Cloud (EC2)
<details>
  <summary>Click to expand!</summary>
  
  ### EC2 User Data
  You can bootstrap instances using EC2 User Data script, which launch commands when a machine starts. This is to automate boot tasks such as:
  * Installing updates
  * Installing software
  * Downloading common files from internet

  ### EC2 Instance Types
  #### General Purpose
  * For diversity of workloads, a balance between compute, memory and networking.
  * Example use cases:
    * Web servers
    * Code repositories

  #### Compute Optimized
  * For compute-intensive tasks that require high performance processors
  * Example use cases:
    * Batch processing workloads
    * Media transcoding
    * High performance web servers
    * High performance computing (HPC)
    * Scientific models & machine learning
    * Dedicated gaming servers

  #### Memory Optimized
  * Fast performance for workloads that process large data sets in memory
  * Example use cases:
    * High performance, relational/non-relational databases
    * Distributed web scale cache stores
    * In-memory databases optimized for BI (business intelligence)
    * Applications performing real-time processing of big unstructured data

  #### Storage Optimized
  * For storage-intensive tasks that require high sequential read/write access to large data sets on local storage
  * Example use cases:
    * High frequency online transaction processing (OLTP) systems
    * Relational & NoSQL databases
    * Cache for in-memory databases (e.g. Redis)
    * Data warehousing applications
    * Distributed file systems

  #### Accelerated Computing
  * Accelerated computing instances use hardware accelerators, or co-processors, to perform functions, such as floating point number calculations, graphics processing, or data pattern matching, more efficiently than is possible in software running on CPUs
  * Instances such as the G and the P instances has GPUs
  * Example use cases:
    * Machine learning
    * High performance computing
    * Computational fluid dynamics
    * Computational finance
    * Seismic analysis
    * Speech recognition
    * Autonomous vehicles
    * Drug discovery

  ### EC2 Instances Launch Types
  #### On-Demand Instances
  * Pay for what you use
  * Highest cost but no upfront payment
  * No long term commitment
  * Recommended for short-term and un-interrupted workloads

  #### Reserved Instances
  * Up to 75% discount compare to On-Demand
  * Longer reservation = more discounts
  * Purchasing options:
    * No upfront
    * Partial upfront (discounts+)
    * All upfront (discounts+++)
  * Reserve a specific instance type
  * Recommended for steady usage applications, one that you know will be running consistently for long term. (e.g. databases)


  Convertible Reserved Instance
  * Can change EC2 instance type
  * Up to 54% discount


  Scheduled Reserved Instance
  * Launch within time window you reserve
  * Recommended for if you only require instance to run on a fraction of day/week/month
  * Commitment still 1-3 years

  #### Spot Instances
  * Most cost efficient, up to 90% discount compare to On-Demand
  * However, you can "lose" the instance at any point of time if your max price is lower than the current spot price
  * Useful for workloads that are resilient to failure:
    * Batch Jobs
    * Data analysis
    * Image processing
    * Any distributed workloads
    * Workloads with flexible start and end time
  * Not suitable for critical jobs or databases
  * How it works is that you define max spot price, and you get the instance while current spot price < max spot price

  Spot Block
  * Block spot instances during specified time frame (1-6 hours) without interruptions
  * In rare situations, instances may be reclaimed

  Spot Fleets
  * Set of spot instances + (optional) On-Demand instances
  * Spot fleet will try its best to meet target capacity with price constraints
  * Define possible launch pools: instance type, OS, Availability Zone
  * Strategies to allocate Spot instances:
    * lowestPrice: from the pool with the lowest price (cost optimization, short workload)
    * diversified: distributed across all pools (great for availability, long workloads)
    * capacityOptimized: pool with the optimal capacity for the number of instances
  * Spot fleets allows us to automatically request Spot instances with the lowest price

  #### Dedicated Hosts
  * Physical server with EC2 instance capacity fully dedicated to your use
  * 3 year period reservation
  * More expensive
  * Useful for software with complicated licensing models or companies with strong regulatory/compliance needs

  #### Dedicated Instances (Take note: different from Dedicated Hosts)
  * Instances running on hardware dedicated to you
  * May share hardware with other instances in same account
  * An important difference between a Dedicated Host and a Dedicated instance is that a Dedicated Host gives you additional visibility and control over how instances are placed on a physical server, and you can consistently deploy your instances to the same physical server over time. As a result, Dedicated Hosts enable you to use your existing server-bound software licenses and address corporate compliance and regulatory requirements.

  ### EC2 Placement Groups
  * To control EC2 placement strategy

  Placement group strategies:
  * Cluster
    * Clusters instances into low latency group in a single AZ
    * For low latency and high network throughput use cases, but risk of all instances failing if rack fails
    * Use cases: Big data job that needs to complete fast, applications that needs low latency and high network throughput
  * Spread
    * Spreads instances across underlying hardware (max 7 instances per group per AZ, for critical applications)
    * Spans across AZs hence reduced risk of failure, maximized high availability
    * Use cases: Critical applications
  * Partition
    * Spreads instances across many different partitions within an AZ, scales to 100 of EC2 instaces per group (Hadoop, Cassandra, Kafka)
    * Can span across multiple AZs.
    * Use cases: HDFS, HBase, Cassandra

  ### Elastic IP
  * EC2 public IP can change if you restart it. If you need a fixed IP for your instance, you need an Elastic IP.
  * Can only have 5 Elastic IP in your account (can ask AWS to increase that)
  * Best practice is to try to avoid using Elastic IP and instead use random public IP and register DNS to it. Or, use Load balancer.

  ### Elastic Network Interfaces (ENI)
  * Logical component in a VPC that represents a virtual network card
  * Can be moved across instances for purposes of failovers

  ### EC2 Hibernate
  * Preserves RAM state
  * Instance boot is much faster
  * Use cases:
    * Long-running processing
    * Saving the RAM state
    * Services that take a lot of time to initialize


  ### Useful Online Tool for Searching Instances Type
  To find info of/compare between EC2 instance types: https://instances.vantage.sh/

</details>

## EC2 Storage
<details>
  <summary>Click to expand!</summary>

  ### Amazon Machine Image (AMI)
  * Works like a Virtual Machine image
  * Built for specific region but can be copied to another region
  * You can create AMI of your existing instances
  * Can consider creating AMI rather than using EC2 User Data, for faster boot

  ### Elastic Block Storage (EBS)
  * Network drive that can be attached to your instances
  * EBS Volume can be detached from one instance and then attached to another, but note that it can only be attached to one instance at any one time
  * Bound to specific AZ, cannot attach to instance in other AZ. To move to another AZ have to snapshot it first
  * Provisioned capacity (GBs, IOPS) but can increase capacity over time

  #### Delete On Termination
  By default for EC2 instances, their root EBS volume has Delete On Termination enabled. In scenarios where you want to preserve the root volume after termination, disable this attribute.

  ### EBS Volume Types
  * gp2/gp3 - General Purpose SSD, balance price and performance
  * io1/io2 - Highest Performance SSD for low latency and high throughput
  * st1 - Low cost HDD for frequently accessed, throughput intensive workloads
  * sc1 - Lowest cost HDD for less frequently accessed workloads

  For performance comparison, best to refer to tables in this link: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html

  ### EBS Multi-attach
  * For io1/io2 family
  * Attach same EBS Volume to multiple EC2 instances in same AZ, with full read/write by all instances
  * Use case:
    * Achieve higher application availability in clustered Linux applications (e.g. Teradata)
    * Application must be able to manage concurrent write operations

  ### EBS Encryption
  * Snapshots of encrypted volumes are encrypted
  * To encrypt an unencrypted volume:
    * Create EBS snapshot of volume
    * Encrypt the EBS snapshot (using copy)
    * Create new EBS volume from snapshot (the volume will also be encrypted)
    * Attach volume to original instance
  
  ### EBS Raid Options
  * RAID 0 (Increase performance, but no fault tolerance)
  * RAID 1 (Increase fault-tolerance)
  * RAID 5 (not recommended for EBS)
  * RAID 6 (not recommended for EBS)

  ### Elastic File System (EFS)
  * Managed NFS that can be mounted on many EC2
  * Works with EC2 instances across AZs
  * Highly available, scales automatically, expensive (3x gp2), pay per use
  * Only for Linux instances

  ### EC2 Instance Store
  * High-performance hardware disk, physically attached to the server, very high IOPS
  * Loses their storage if stopped, and risk of data loss if hardware fails
  * Good for buffer/cache/scratch data/temporary content

</details>

## Security Groups
<details>
  <summary>Click to expand!</summary>

  ### Security Groups
  * Controls traffic in/out of attached objects
  * Only contains allow rules
  * Can reference by IP CIDR
  * Can reference by another security group, e.g. Only instances with "Nginx" Security groups can communicate with instances from "Django" security groups via HTTPS port 443
  * All inbound traffic is blocked by default
  * All outbound traffic is authorised by default

  ### Additional Notes
  * If your application is inaccessible (time out), could possibly be security group issue
  * If your application gives a "connection refused" error, likely to be application error or not launched, rather than security group issue
</details>

## IAM
<details>
  <summary>Click to expand!</summary>
  
  ### IAM Best Practices
  * Don't use root account except for AWS account setup
  * One physical user = One AWS user
  * Assign users to groups and assign permissions to groups
  * Create strong password
  * Use and enforce the user of Multi Factor Authentication (MFA)
  * Create and use roles for giving permissions to AWS services
  * Use Access Keys for Programmatic Access (CLI/SDK)
  * Audit permissions of your account with the IAM Credentials Report
  * Never share IAM users & Access Keys

  ### Access Advisor
  In the IAM Service you can view a user's recently accessed services via Users > Select Users > Access Advisor Tab. This should give a quick overview of services that the user has permissions for, but is unused by the user. You can use this to determine if unnecessary permissions should be removed.

</details>

