# Web Application Hosting in the AWS Cloud

Notes form this [whitepaper](http://media.amazonwebservices.com/AWS_Web_Hosting_Best_Practices.pdf).

### "Traditional" Web Hosting

![Traditional Web Hosting](-Jj_ft088CTEx0a_V_i6)

### Capacity Optimization

![Capacity Optimization](-Jj_gX90j52b5dWvGOiN)

### AWS Web Hosting

![AWS Web Hosting](-Jj_i4kPEhLwv1WMwnuV)

### Security Groups

> Amazon Elastic Compute Cloud (EC2) provides a feature called security groups. A security group is analogous to an inbound network firewall, for which you to specify the protocols, ports, and source IPs
ranges that are allowed to reach your EC2 instances. Each EC2 instance can be assigned one or more security groups,
each of which routes the appropriate traffic to each instance. Security groups can be configured so that only specific subnets or IP addresses have access to an EC2 instance, or they can reference other security groups to limit access to EC2 instances that are in specific groups.

![Security Groups](-Jj_j_zq8Yy2DAxiuXmU)

### IP Addresses

> In the traditional web hosting architecture, most of your hosts have static IP addresses; in the cloud, most of your hosts
will have dynamic IP addresses. Although every EC2 instance can have both public and private DNS entries and will be
addressable over the Internet, _the DNS entries and the IP addresses are assigned dynamically when you launch the instance_, and they cannot be manually assigned. Static IP addresses (Elastic IP addresses in AWS terminology) can be
assigned to running instances after they are launched. You should use Elastic IP addresses for instances and services that require consistent endpoints, such as, master databases, central file servers, and EC2-hosted load balancers

### Databases

![Databases](-Jj_m9MhVE2KpAI3wEUR)

Hosted solution provides many DBA features, such as back-ups, patches, etc. There are also additional features for increased availability and read-only replicas.

For self-hosting on EC2, an Amazon Elastic Block Storage (EBS) is recommended to decouple storage from the host machine.

EBS also supports snapshotting, which can be storage in S3.

> To maximize the performance of your I/O-intensive applications, you can use _Provisioned IOPS volumes_. Provisioned IOPS volumes are designed to meet the needs of I/O-intensive workloads,
particularly database workloads that are sensitive to storage performance and consistency in random access I/O throughput. You specify an IOPS rate when you create the volume and Amazon EBS provisions that rate for the lifetime of the volume. Amazon EBS currently supports up to 1,000 IOPS per volume. You can stripe multiple volumes together to deliver thousands of IOPS per instance to your application.

### Failover Management

> Another key advantage of AWS over traditional web hosting is the Availability Zones that give you easy access to
redundant deployment locations. _Availability Zones are physically distinct locations that are engineered to be insulated
from failures in other Availability Zones_. They provide inexpensive, low-latency network connectivity to other Availability
Zones in the same Region. As the AWS web hosting architecture diagram in this paper shows, we recommend that you
_deploy EC2 hosts across multiple Availability Zones to make your web application more fault-tolerant_. Its important to
ensure that there are provisions for migrating single points of access across Availability Zones in the case of failure. For
example, a database slave should be set up in a second Availability Zone so that the persistence of data remains
consistent and highly available even during an unlikely failure scenario. 

> _Availability Zones within an AWS region should be thought of as multiple datacenters_. EC2 instances in different
Availability Zones are both logically and physically separated, and they provide an easy-to-use model for deploying your
application across data centers for both high availability and reliability.
----

[Source](http://media.amazonwebservices.com/AWS_Web_Hosting_Best_Practices.pdf) 