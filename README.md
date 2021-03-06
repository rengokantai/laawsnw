# laawsnw
##1. AWS Network Foundations
###1 Understanding Virtual Private Cloud (VPC)
The route table in your public subnet needs to be point to the internet gateway. Similarly, the route table in your private subnet needs to point to the NAT Gateway in your public subnet.

###2 Establish private connections
One way to connect your AWS account with your existing facilities is by establishing an Internet Protocol Security, or IPsec, VPN tunnel.

####03:47
Keep in mind that VPC peering connections only work within the same AWS region.


###3 Understanding Route 53









##2. Virtual
###1 Explore the default VPC
####04:14 default vpc name
One of the reasons the name tag is so special is that once assigned it permeates throughout the web console. 



###2 Explore VPC templates
####01:50  
Looking at the text in the center, we see that this wizard would create a VPC with a /16 network that will also crate a /24 subnet containing 251 available IP addresses.  
Why only 251 instead of 254? This is because AWS reserves the first three addresses for internal communication needs.  


####02:24  
For /24 address, we have 256 addresses, 254 Hosts



###4 Configuring VPC subnets
####04:49
```
aws ec2 create-subnet --dry-run --vpc-id vpc-12345678 --cidr-block 10.0.1.0/24 --availability-zone us-west-1a --profile ke
```
create a tag
```
aws ec2 create-tags --resources subnet-12345678 --tags Key=k,Value=v --profile ke
```


###5 Configuring route tables
Main route table can be modified but not deleted.  
####02:56 create routetable
```
aws ec2 create-route-table --vpc-id vpc-12345678 --profile ke
```
associate
```
aws ec2 associate-route-table --route-table-id rtb-12345678 --subnet-id subnet-12345678 --profile ke
```
tags
```
aws ect create-tags --resources rtb-12345678 --tags Key=k,Value=v --profile ke
```


###6 Configuring an Internet gateway
####04:10 create igw
```
aws ec2 create-internet-gateway --profile ke
```
add tag
```
aws ec2 create-tags --resources igw-12345678 --tags Key=k,Value=v --profile ke
```
attach to vpc
```
aws ec2 attach-internet-gateway --internet-gateway-id igw-12345678 --vpc-id vpc-12345678 --profile ke
```
create route
```
aws ec2 create-route --route-table-id rtb-12345678 --desination-cidr-block 0.0.0.0/0 --gateway-id igw-12345678 --profile ke
```



###7 Configuring a NAT gateway

NAT instance: support port forwarding,support bastion host    
NAT gateway: does not support port forwarding, does not support bastion host   


####03:20 create NAT gateway in public subnet.

####05:23 
then in parivate route table, add destnation=0.0.0.0/0, target=nat gateway



###8 Configuring VPC endpoint for S3
While S3 is the only currently available VPC endpoint,AWS has stated that it is working on creating additional types of VPC endpoints.  

####01:39 create endpoint
IAM policy  
The question is what type of access do I want to allow incidences within my VPC to have to S3.

















##3. VPC Security
###1 Understanding security groups
####01:00
500/50/5 rule 
There are 5 security groups allowed per network interface. There are 50 rules allowed per security group.  
And finally, there are 500 security groups allowed in a given VPC.  

####02:38
security groups are stateful. Responses to any outbound connection will always be allowed, regardless of any rules limiting inbound traffic.


###2 Configuring security groups



###3 Understanding network ACLs
NACLs, are a tool for controlling how traffic flows into, and out of, subnets within a VPC

First off, NACLs exist within the confines of a single VPC, and do not span VPCs. If you want to use the same NACL ruleset in multiple VPCs, you'll have to configure the NACL in each VPC. Within a VPC, NACLs can be associated with one, or more, subnets. However, each subnet can only be associated with one, and only one.

###5 Configuring network ACLs
####05:20
Since NACLs are stateless, when creating an inbound rule, must also create same outbound rule


###6 Understanding VPC flow logs
VPC Flow Logs are a way to log network traffic associated with a VPC.



####01:50
You can't create a Flow Log for a peered VPC connection, unless that VPC is in the same AWS account. Once a Flow Log is created, its configuration cannot be altered. If you want to alter a Flow Log's configuration, the only way to do it is by creating a new Flow Log and deleting the old one. 

###7 Explore VPC Flow Logs
####00:43
cloudwatch->logs->create log group  

####02:45
put destination log group with the group we just created.  


##4. VPC Peering
###2 Implement VPC peering
###3 Configure VPC peer routing
####02:30
If vpc1's pri want to access vpc2's pub and pri, in vpc1's pri's route table,add  
dest->vpc2's cidr, target->pcx-xxx  


####03:53
add route in vpc2's public subnet's route table in order to connect vpc1's pri subnet.  


add route in vpc2's private subnet's route table in order to connect vpc1's pri subnet.  




##5. Route 53
###2 Use Route 53 alias
####00:20
In regular DNS, you use IP addresses and domain names.  
A Route 53 alias can be configured to point to an S3 bucket that has been configured to be available as a static website.  
This also allows you to host a static website in S3 and manage its domain in Route 53.   
S3/ELB/cloudfront/el bean
####03:10
website hosting  
permissions->add Grantee:everyone, grant view permissions  

####4:56 set A type
alias->yes  
alias target: s3 bucket


###3 Understand private DNS
###4 Configure private DNS
###02:57
create a new hosted zone, enter domain name, type=privat hosted zone. set to true for
```
enableDnsHostname
enableDnsSupport
```
####04:15 create record set  
alias: no  
value: private ip of ec2

####05:07
private hosted zones in Route53 are aeasy to cofig without affecting production


###6 Using an elastic IP and Route 53
####02:30 create a record set.  
type->A  (replace previous s3 route)
alias: no
value: elastic ip
