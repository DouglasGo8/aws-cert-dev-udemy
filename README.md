### Ultimate AWS Certified Developer Associate 2020
<br />

<ol>
    <li>
        Each region has many availability zones, 
        usually 3, min 2 and max 6
        e.g - <strong><em>ap-southeast-2a | ap-southeast-2b</em></strong> on link
        [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/?p=ngi&loc=2).
        <br />
        [AWS Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/?p=ngi&loc=4).
    </li>

</ol>

#### Terraform Commands
EC2 Key Pairs Instance
```
$ ssh-keygen -f {{FILE_NAME}}.pem
$ sudo chmod 600 /path/{{FILE_NAME}}.pem
$ sudo chmod 755 /PEM_PATH
$ ssh -i {{PEM_FILE}} ec2-user@{{PUBLIC_IP}}
$ ssh -i {{PEM_FILE}} ubuntu-user@{{PUBLIC_IP}}
$ ifconfig -a
```

#### IAM (Identity and Access Management)
 * Users
 * Groups
 * Roles  
##### Root Account (Never be used or shared)


#### EC2
 * Renting virtual machines (EC2)
 * Storing data on virtual drives (EBS)
 * Distributing load across machines (ELB)
 * Scaling the services using an auto-scaling group (ASG)
 * Pricing is per hour
   * Region in
   * Instance Type
   * Launch Instance
   * OS Type
   * R/C/P/G/H/X/I/F/Z/CR are spec in RAM, CPU, I/O, Network, GPU
   * M instances types are balanced (no good fo GPUs)
   * T2/T3 types are 'burstable' (free) can have a spike and a CPU can keep very good
  
#### EC2 Instance Launch Type
 * Reserved 
   * Reserved minimum 1year 
   * Convertible Reserved
   * Scheduled Reserved Instances
 * Spot Instances
   * short workloads for cheap but can loose instances
 * Dedicated Instances
 * Dedicated Hosts

#### Security Groups
 * Are the fundamental of network security in AWS
 * They control how traffic is allowed into or out in ec2 vm 
 * Acting as a "firewall" on EC2 instances
 * Actually
    * Access to Ports
    * Authorize IP ranges - IPv4 and IPv6
    * Control of inbound network (from other to the instance)
    * Control of outbound network (from the instance to other)
    * Can be attached to multiple instances
    * Locked down to a region /VPC combination
    * Switched new region? Must have new SG or another VPC
    * It's good to keep one ***separate SG group for SSH access***
    * Application it's not accessible (time out), then we have a SG issue
    * Default behavior to SG is Deny all traffic inbound and allow all traffic outbound

#### Private vs Public IP (IPv4)
* Public IP
  * Means the machine can be identified over internet (www)
  * Must be unique across the whole web (not two machines have the same IP)
  * Can be geo-located easily
* Private IP
  * Means the machine can only be identified on a private network only
  * The IP must be unique across the private net
  * Two different companies networks can have the same IPs
  * Machines connect over WWW using a NAT + INTERNET GATEWAY (a proxy)
* Elastic IPs
  * When you stop and then start an EC2 instance, it can change its public IP
  * When you need to have a fixed public IP for your instance, you need and Elastic IP  
  * Try avoid use Elastic IP  

#### Scalability
  * Vertical
    * Means increasing the size of the instance
      * App runs over t2.micro => vertically means running over t2.large
      * Commonly in distributed systems, such a DB e.g RDS, ElastiCache
  * Horizontal
    * Means increasing the number of instances / systems for your app
      * Commonly in web apps
      * Easily to scale with EC2
  * High Availability
    * Means usually goes hand in hand with horizontal scaling
    * Vertical increasing instance size
    * Horizontal Increase number of instances (Auto Scale Group|Load Balance)
    * Running multiple instances of app across multi AZ

#### Load Balancing
  * Are servers that forward internet traffic to multiple EC2 instances
  * Why Use LBs
    * Spread load across multiple downstream instances
    * Expose a single point of access through DNS to your app
    * Seamlessly handle failures of downstream instances
    * Do regular health checks to your instances
    * Provide SSL termination HTTPS to your websites
    * Enforce stickiness with cookies
    * High availability across zones
    * Separate public traffic from private traffic
    * EC2 ELB Load Balance is a auto managed Load Balancer
  * Health Checks
    * Are crucial for Load Balancers, overall it is recommended to use the newer / v2 generation of load balancers
    * We can setup internal (private) or external (public) ELBs
  * Classic Load Balancers
    * Support TCP (Layer 4), HTTP & HTTPS (Layer 7)
    * Health checks are TCP or HTTP based
    * Fixed hostname xxx.region.elb.amazonaws.com
  * Application Load Balancer (v2)
    * Application load balancers is Layer 7 (HTTP)
    * Load balancing to multiple HTTP applications across machines
    * Load balancing to multiple applications on the same machine (ex. containers)
    * Support HTTP/2 and WebSocket
    * Support redirects (from HTTP to HTTPS)
    * Routing tables to different target groups
      1. Routing based on path in URL (foo.com/***users*** & foo.com/***posts***)
    * Great fit with Micro Services * Container-based app e.g Docker & Amazon ECS