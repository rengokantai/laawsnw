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
