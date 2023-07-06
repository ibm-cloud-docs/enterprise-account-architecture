---

copyright:
  years: 2023
lastupdated: "2023-06-29"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Cloud financial management
{: #finops}

Cloud financial management is the cultural practice of extracting more value out of cloud resources by holding teams within your enterprise accountable for their cloud costs rather than being managed at the account level by the finance department.
{: shortdesc}

Enabling teams to regularly monitor their cloud spend helps teams drive business decisions that improve infrastructure efficiency. These best practices are intended to help start and mature your enterprise's [finops capabilities](https://www.finops.org/framework/capabilities/).

## Cost allocation
{: #cost-allocation}

Projects have built in resource tagging mechanics that ensure all resources within the enterprise are tagged with appropriate project tags. When combined with {{site.data.keyword.cloud_notm}} usage reporting by tag, enterprises can rollup all your cloud costs across many {{site.data.keyword.cloud_notm}} accounts to your project teams.

Further cost allocations can be accomplished by tagging projects rather than individual cloud resources and allocating costs to organizational units or cost centers. From here, chargebacks and showbacks can readily be accomplished.

By using enterprise separation between nonproduction and production environments, project costs can be allocated between R&D and Cost of Goods accounting categories.

Because of the way projects tag resources, deallocated resource costs can be identified by using the Security and Compliance Center by scanning for untagged resources, indicating manually provisioned resources. Any untagged resources that are found indicate a gap in an access control policy and improper configuration management that should trigger a root cause analysis.

## Resource utilization and efficiency
{: #resource-utilization-and-efficiency}

{{site.data.keyword.cloud_notm}} monitoring can be used to monitor resource utilization across your various compute instances by allowing your enterprise to identify infrastructure right-sizing opportunities or to assess whether the performance, availability, or other quality metrics are of value for the expense incurred.

Centralizing monitoring can also be used to identify infrastructure consolidation opportunities where multiple applications can be on bare minimum hardware yet struggle to efficiently use the resources available. Consolidating the hosting of many small applications can minimize over-provisioning and improve availability through added redundancy.
