# AWS-Route53

## Overview
- In AWS DNS is in the form of Route 53.
- It is used to resolve domain names to IP addresses so request can retrieve the relevant service/application/information.
- Typically in real world networks, we have a DNS server, which is able to carry out this function.
- However, Route 53 is more advantageous integrates directly with AWS services and provides DNS management, domain registration, health checks, and multiple         routing policies.
- Route 53 allows domain registration, so you don't have to purchase a domain off a third party platform
  and also allows existing domains to be easily imported.
- In addition, traditionally businesses would be required to find a hosting platform to host their application.
- When using AWS Route 53, you don't have to worry about that as all the utilities/resources needed are on AWS itself.
- There are also Hosted Zones, which allow you to add/update/edit DNS records.
- Health checks are also synonymously carried out to ensure everything is operating normally.

## Architecture

<img width="1100" height="607" alt="image" src="https://github.com/user-attachments/assets/97cd5cd4-cf9c-47d9-99e6-5cb04a434c54" />

In the architecture above, you can see that first a user searches for a webpage via the World wide web. Then Route 53 takes care of the request by resolving the Domain name to the related IP address- the request is then forwarded to the relevant resource via http/https. If a front-end application was hosted on an ec2 instance, a DNS "A record" would be configured to point all requests to the EC2 instance hosting the application. Realistically, there would be a few instances hosting the application for fault tolerance and to avoid a single point of failure. Also a Load balancer would be at the fore front of the instances and the domain name would be tied to that. Furthermore, a more secure solution- to prevent overloading servers would be implemented such as Weighted Routing. 

## Traffic Flow
<img width="237" height="385" alt="image" src="https://github.com/user-attachments/assets/33d9a0f1-7211-4630-9ec5-51f085e94edb" />

## Weighted Routing
<img width="1082" height="593" alt="image" src="https://github.com/user-attachments/assets/6b19c79a-f2e9-4ccd-9806-d2dee7447ed0" />

In this architecture, you can see that there are multiple load balancers pointing to EC2 instances. (each load balancer is pointing to one EC2 instance for 
simplicity. There would be more instances realistically) In addition to this, the arrows have a percentage which signifies the load they are configured to handle.
This means that inbound traffic will be shared among 2 instances- each handling approximately half of the requests.

This architecture is far more flexible and available than the first architecture, as there 2 load balancers, so if one happens to become overloaded or unavailable the second can continue to host requests- making the application highly available. 

## Traffic Flow

                         Route 53
                    Weighted Routing
                      /          \
                   50%            50%
                    │               │
                    ▼               ▼
                  ALB A           ALB B
                    │               │
               Target Group    Target Group
                    │               │
                  EC2s            EC2s

## Advantages
- Canary deployments
- Blue/green deployments
- Testing a new application version
- Gradually shifting traffic
- Running applications in different environments/regions

## Failover Routing
<img width="1121" height="607" alt="image" src="https://github.com/user-attachments/assets/bb6d11bc-4474-4a45-b917-cd5301c53e20" />

In this architecture, the main concern/ priority is availability. Many organisations prioritise availability over anything else. This a simplistic layout on how organisations are able to achieve this in AWS through configurations in Route 53. As you can see there are only 2 instances. (each after a load balancer) In route 53 you configure a primary resource and a secondary resource. If the primary resource happens to go down or become unavailable, traffic is rerouted to the available resource to maintain availability. 

##Traffic Flow

                         Route 53
                     Failover Routing
                            │
                       Health Check
                            │
                     ┌──────┴──────┐
                     │             │
                  PRIMARY       SECONDARY
                     │             │
                     ▼             ▼
                   ALB-A         ALB-B
                     │             │
                   EC2s          EC2s
## Advantages
- High Availability
- Removes single fault of failure
- Automatic Failover
- Reduced Downtime
- Geographic Resilience
