---
title: Developing in the Cloud
theme: sky
highlight-theme: androidstudio
template: template.html
revealOptions:
    transition: 'concave'
---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2015/09/09/19/08/sky-932661_1280.jpg" data-background-opacity=".5"  -->
# Developing in the cloud
Tim Goff \
timothy.d.goff@raytheon.com \
timothy.d.goff@nasa.gov

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2015/09/05/20/02/coding-924920_1280.jpg"  data-background-opacity=".2"  -->
## Who am I?
- Tim Goff
    - Earthdata Cloud Senior Developer
    - Telecommute from the beautiful Blue Ridge Mountains of VA
    - 16 years with Raytheon - All on NASA EED Contract
        - Started with legacy C++ applications
        - Moved to green field Java backend development
        - Ruby and javascript search application
        - Clojure (LISP on the JVM) metadata catalog development
    - Last few years
        - Maintain legacy PaaS system
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

---
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

---
## Infrastructure as Code
- Source control your whole infrastucture
- Repeatable and recoverable architecture
- Everything except for data is Cattle 
- Cloudformation - https://aws.amazon.com/cloudformation/
    - Built in to AWS
- Terraform - https://www.terraform.io/
    - Multi cloud
    - More dyanmic DSL

---
## IaC Examples

```terraform
resource "aws_vpc" "app" {
  cidr_block           = var.vpc_cidr_range
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name                   = "Application VPC"
    gsfc-ngap-prot-managed = "true"
  }
}

resource "aws_subnet" "private_app" {
  # private subnets in different AZs
  count                   = var.num_private_subnets
  vpc_id                  = aws_vpc.app.id
  map_public_ip_on_launch = false
  availability_zone       = element(data.aws_availability_zones.available.names, count.index)

  # see https://fishingcatblog.wordpress.com/2016/09/22/terraform-interpolation-cidrsubnet/ for more on `cidrsubnet`
  cidr_block = cidrsubnet(var.vpc_cidr_range, 2, count.index)

  tags = {
    Name                   = "Private application ${data.aws_availability_zones.available.names[count.index]} subnet"
    gsfc-ngap-prot-managed = "true"
  }
}
```

---
## CICD

---
<!-- .slide: data-background-image="https://cdn.pixabay.com/photo/2016/12/01/01/32/blue-ridge-1874266_1280.jpg"  data-background-opacity=".2"  -->
## What should you take from here?

- Play in a cloud env
- Embrace Infrastructure as Code
- Utilize cloud native concepts where possible
- Follow people on twitter e.g Corey Quinn
- Reinvent: free this year https://reinvent.awsevents.com/
- ACloudGuru - https://acloudguru.com/ 

