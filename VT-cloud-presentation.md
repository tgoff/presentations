---
title: Developing in the Cloud
theme: sky
highlight-theme: androidstudio
template: template.html
revealOptions:
    transition: 'concave'
---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2015/09/09/19/08/sky-932661_1280.jpg" data-background-opacity=".3"  -->
# Developing in the cloud
Tim Goff \
timothy.d.goff@raytheon.com \
timothy.d.goff@nasa.gov

---
<!-- .slide: data-background-image="img/IMG_20200826_193120.jpg" data-background-opacity=".5"  -->

## Who am I?
- Tim Goff
    - Earthdata Cloud Senior Developer
    - Work from home in the beautiful Blue Ridge Mountains of VA
    - 16 years with Raytheon - All on NASA EED Contract
        - Started with legacy C++ applications
        - Moved to greenfield Java backend development
        - Ruby and javascript search application
        - Clojure metadata catalog development
        - Legacy PaaS for Earthdata Applications
    - Currently
        - Develop and maintain IaaS reimplementation

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2017/06/14/16/20/network-2402637_1280.jpg"  data-background-opacity=".2"  -->
## PaaS vs IaaS - Review
- PaaS EDC - managed by Ruby backend and API
    - V1: Applications sent in tar files.  Platform expanded and ran on an EC2 instance
    - V1.1: Applications sent in Docker containers, platform initiated ECS services
- Iaas EDC
    - Applications have the keys to full(*) AWS experience
    - Guardrails built to ensure applications are compliant, secure, and app owners cant do things they shouldnt be able to
- Why IaaS?
    - Allows cloud native application development using most of the growing suite provided by AWS
    - Developers want to be able to use the full potential of the cloud

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2015/09/05/20/02/coding-924920_1280.jpg"  data-background-opacity=".2"  -->
## Developing for the cloud
- 12 factor app - https://12factor.net/
    - Simple, standalone services which do not have dependencies baked in
- Microservices
    - Multiple small independent services instead of a monolithic application
    - Indepdendently scalable services
- Serverless
    - Lambda
    - ECS/EKS
- Cattle vs Pets
- Managed services
    - DB, CDN, etc
    - Fargate vs self managed ECS  

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2015/09/29/22/49/blueprint-964630_1280.jpg"  data-background-opacity=".2"  -->
## Infrastructure as Code
- Source control your whole infrastucture
    - Including networking
- Repeatable and recoverable architecture
- Everything except for data is Cattle 
- Cloudformation - https://aws.amazon.com/cloudformation/
    - Built in to AWS
    - YAML or JSON syntax
- Terraform - https://www.terraform.io/
    - Multi cloud
    - More dynamic JSON-like DSL (HCL)

---
## IAC examples
### Terraform
```terraform
resource "aws_vpc" "app" {
  cidr_block           = var.vpc_cidr_range
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name                   = "Application VPC"
  }
}

resource "aws_subnet" "private_app" {
  # private subnets in different AZs
  count                   = var.num_private_subnets
  vpc_id                  = aws_vpc.app.id
  map_public_ip_on_launch = false
  availability_zone       = element(data.aws_availability_zones.available.names, count.index)
  cidr_block = 10.0.0.0/16

  tags = {
    Name                   = "Private application ${data.aws_availability_zones.available.names[count.index]} subnet"
  }
}
```

### CloudFormation
```yaml
myVPC:
  Type: AWS::EC2::VPC
  Properties:
    CidrBlock: 10.0.0.0/16
    EnableDnsSupport: 'false'
    EnableDnsHostnames: 'false'
    InstanceTenancy: dedicated
    Tags:
     - Key: foo
       Value: bar
```

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2018/02/15/18/29/devops-3155972_1280.jpg"  data-background-opacity=".2"  -->

## CICD

- As with any codebase, Pull Requests need to be tested prior to merge
- Infrastructure is harder to test than application code
- Where possible - test lambdas locally
- Spin up and test containers in CI environment
- Lambdas and canaries to test infrastructure
- Monitoring of key metrics/logs
- AWS CodeStar suite or other SaaS offerings (e.g. Jenkins)
- Use Config as Code to define buld process

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2016/12/01/01/32/blue-ridge-1874266_1280.jpg"  data-background-opacity=".2"  -->
## Next steps/Resources

- Set up a free tier (AWS/Azure/GCP/etc) account
- Embrace Infrastructure as Code
- Utilize cloud native concepts where possible
- Follow people on twitter e.g @QuinnyPig, @jeffbarr
- Reinvent: Virtual and Free this year! https://reinvent.awsevents.com/
- Pursue cloud certification.  AWS probably more in demand, but Azure seems to be gaining stature.
- ACloudGuru: Excellent certification prep, some of it free https://acloudguru.com/ 

