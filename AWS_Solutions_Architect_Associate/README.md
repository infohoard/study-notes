# AWS Solutions Architect Associate Study Notes

## EC2
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

  ### Useful Online Tool for Searching Instances Type
  To find info of/compare between EC2 instance types: https://instances.vantage.sh/

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

