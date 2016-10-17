# laawsnw
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
