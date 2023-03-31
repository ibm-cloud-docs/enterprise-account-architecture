---

copyright:
  years: 2023
lastupdated: "2023-03-30"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Financial operations
{: #finops}

Financial operations for cloud is about identifying unmanaged resources, allocating costs accurately, and minimizing resource over-provisioning.
{: shortdesc}

## Identifying unmanaged resources
{: #identify-unmanaged-resources}

Cloud as code and IBM Cloud projects are used to ensure that all resources within the enterprise are tagged with the appropriate project. The Security and Compliance Center can be used to scan for untagged resources, indicating manually provisioned resources. Any untagged resources that are found indicate a gap in an access control policy and improper configuration management that should trigger a root cause analysis.

## Allocating costs
{: #allocate-costs}

Given the robust tagging mechanisms through cloud as code and IBM Cloud usage reporting by tag, all costs can be allocated to a project. Allocating costs to a project provides a higher level, more granular unit of cost allocation that eases the job of associating costs with organizational units, cost centers, and so on. These cost allocations can be accomplished by further tagging projects and by using the project account and account group information. From here, chargebacks and showbacks can readily be accomplished. Futhermore, within a project, it is easy to allocate costs between R&D and Cost of Goods accounting categories that use the enterprise separation between nonproduction and production.

## Minimizing resource over-provisioning
{: #minimize-resource-over-provisioning}

This recommendation creates a framework for using shared resources and shared application hosting infrastructure. Shared resources and application hosting infrastructure is an essential ingredient to enable efficient use of infrastructure and allow minimum over-provisioning. Further, the IBM Cloud tools that are included in many of the IBM supplied deployable architectures provide robust tooling for monitoring resource usage, and thus support capacity planning and infrastructure right-sizing. An example of one of these tools is IBM Cloud Monitoring.
