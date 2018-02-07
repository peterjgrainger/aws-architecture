+++
name = "Amazon Elastic Container Service"
date = "2018-02-07T05:26:11Z"
title = "adapter containers"
genre = ""
bio = ""

+++

```
Title: amazon elastic container service
Author: Peter Grainger<peter.grainger1@dwp.gsi.gov.uk>
Version: 0.1.0
Audience: All Architects, Principal Software Engineers, Devops, Software Engineers
```

# amazon elastic container service

## Context
Containerisation and the benifits are mentioned in [containerisation](containerisation.md).  The amazon elastic container service deals with the orchanstration of these containers.

## Problem
There are multiple tasks associated with running containers.  Restart on a fatal error, scaling to meet demand, communication between services, allocation between servers, maintaining availablity, patching.  These tasks are difficult and time consuming to do manually and introduce risks of human error. 

## Motivation / Forces / Influences
* **Reliability** To maintain acess to the application the system needs to be self healing and scaling to maintain uptime.
* **Maintainability** architecture to be self healing and automatically upgraded to minimise resources and avoid downtime due to upgrade of systems
* **Cost effective** resources to be allocated wisely to minimise costs
* **Availablity** downtime to be minimised during working hours  

## Solution

### Overview

ECS configuration requires configuration of a cluster containing one or more tasks.  Each task can hold one or more containers.

![](https://s3.amazonaws.com/compute-blog/images/alb-ecs.png)

### Routing 

Routing is done through an application load balancer based on route.  e.g. path `/api/` would be routed to the api task

### Container registry

Images would be versioned and stored in the gitlab registry.  See [Container Image Management](container-image-management.md)

### Patching

Patching would be achived by updating the image version in the terraform script and cloud services applying the terraform architecture.  A further improvement would be to have a `latest` version that tracked the master branch and the trigger for updating the version across the cluster could be achieved through the pipeline.

### Security

Secure info would include: TLS keys, encryption keys etc.  Suggest to hold this in KMS and use within the task

![](https://raw.githubusercontent.com/awslabs/ecs-secrets/master/diagrams/Create.png)


### Advantages

* Reduce downtime due to failures in the system as it is self healing and automatically scaling
* Minimise staff resource cost with easy integration with [KMS]<https://aws.amazon.com/kms/> and [Cloudwatch]<https://aws.amazon.com/cloudwatch> 
* Minimise resource cost and downtime due to self healing and automatic upgrades
* Minimise vm costs as multiple containers will be assigned to one vm.

### Disadvantages

* Moving to a new provider would mean re-evaluating the best system to use.

## Alternatives

### Azure container service

#### Advantages

Integration with third party kubernetes package mangement system [Helm]<https://github.com/kubernetes/helm>

#### Disadvantages

Currently no work has been done with Azure and connectivity with DOI will not be introduced in the near future.

### Kubernetes

[Kubernetes]<https://kubernetes.io/> is currently the most popular orchanstration tool and is starting to be supported as a managed service on multiple cloud providers

### Amazon managed

[EKS](https://aws.amazon.com/eks/) is still in preview.

### Self managed

It is possible to configure kubernetes using ec2 servers directly

#### Advantages

Fully configurable

#### Disadvantages

There isn't experienced resource available to configure, the clusters would need to be maintained, failures investigated and software upgrades may incur downtime.


## Related Patterns
* [Containerisation](containerisation.md) 
* [Container Image Management](container-image-management.md)
