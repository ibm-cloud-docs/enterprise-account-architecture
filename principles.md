---

copyright:
  years: 2023
lastupdated: "2023-04-10"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Principles
{: #principles}

This recommendation uses the following key principles to obtain scale while optimizing for compliance, cost, and operational efficiencies.
{: shortdesc}

- Manage cloud as code (compliance and ops optimization)
- Separate development and production environments (general compliance)
- Hub and spoke (networking optimization)
- Avoid duplication of shared tools (cost and ops optimization)
- Use shared infrastructure for application workloads (cost and ops optimization)
- Separate workload and management (compliance)
- Work within IBM Cloud operating limits and constraints

## Cloud as code
{: #iac}

A key part of this recommendation is to do all cloud setup and configuration as code - that is, automate everything from application infrastructure to account creation, to compliance monitoring. Automating everything allows the enterprise to ensure scale, compliance, security, and so on, in a repeatable and easily evolvable fashion. IBM Cloud provides a number of deployable architectures in the IBM Cloud catalog that can be used to automate these recommendations with minimal effort. These pre-made solutions can also be customized or extended to match the needs of the enterprise.

Managing infrastructure as code at scale requires more than just Terraform in a Git repo. Infrastructure as code is the DNA of an enterprise-wide cloud solution because it must be subject to both governance and management. Infrastructure as code ensures that only validated, compliant, and approved code is deployed. You can also track how deployments correspond to internal projects, teams, cost centers, and more.

IBM Cloud provides a number of tools to assist with this process. The IBM Cloud catalog allows an enterprise to advertise approved IaC solutions to users across the cloud. [IBM Cloud projects](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects) allow an enterprise to ensure that only approved IaC is deployed and to track how the resulting resources correspond to projects, teams, and cost centers.

### Related controls in IBM Cloud Framework for Financial Services
{: #iac-controls}

The following [IBM Cloud Framework for Financial Services controls](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.

| Family | Control |
|--------|---------|
| Configuration Management (CM) | [CM-2 Baseline Configuration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-2) |
| | [CM-2 (2) Automation Support for Accuracy / Currency](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-2.2) |
| | [CM-3 Configuration Change Control](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-3) |
| Access Control (AC) | [AC-5 Separation of Duties](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-5) |
{: caption="Table 1. IBM Cloud Framework for Financial Services controls" caption-side="bottom"}

## Separate development and production
{: #dev-and-prod}

Development environments must be separated from production environments as they have different accounting, resource utilization, access, availability, networking, and compliance requirements. However, at the same time development environments should be kept in sync with production environments so that users testing in development can be confident that the application will behave similarly in production.

This recommendation creates a clean separation between development and production environments, but uses infrastructure as code to ensure that development and production are aligned.

### Related controls in IBM Cloud Framework for Financial Services
{: #dev-and-prod-controls}

The following [IBM Cloud Framework for Financial Services controls](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Configuration Management (CM) | [CM-3 (2) Configuration Change Control &#124; Testing, Validation, and Documentation Of Changes](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-3.2) |
| | [CM-4 (1) Impact Analyses &#124; Separate Test Environments](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-4.1) |
| System and Services Acquisition (SA) | [SA-10 Developer Configuration Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-10) |
| | [SA-15 (9) Development Process, Standards, and Tools &#124; Use of Live Data](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-15.9) |
| System and Communications Protection (SC) | [SC-2 Application Partitioning](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-2) |
| | [SC-3 Security Function Isolation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-3) |
{: caption="Table 2. IBM Cloud Framework for Financial Services controls" caption-side="bottom"}


## Hub and spoke model
{: #hub-and-spoke}

In cloud, it is common to talk about using a hub and spoke model for networking. In this model, a centralized network hub links many spoke networks (VPCs) together. A centralized hub is typically used to reduce costs and increase scale while still allowing private network connectivity within the cloud.

In this recommendation, we also generalize this hub and spoke pattern, applying it to several different areas of concern including networking, managing infrastructure as code, secrets, shared services, and more.


## Shared application infrastructure
{: #shared-infrastructure}

Creating secure, compliant, robust, and scalable application infrastructure can be difficult to set up and costly to maintain. VMs or clusters must be deployed, logging and monitoring configured, firewalls and vulnerability checks enabled, and all of this must be monitored and kept up to date. In addition, this infrastructure must be scaled to accommodate application burst loads and as a result some aspects of the infrastructure are underutilized most of the time.

Standardizing application infrastructure where possible, and then hosting many applications on the same infrastructure, is essential to reduce operations costs. This set up allows the infrastructure to be better utilized and shares the operations cost across many applications. This also makes it practical to use infrastructure as code to deploy many copies of the infrastructure, amortizing the development and maintenance cost of that infrastructure as code across potentially thousands of applications.

As shown in the volumetric analysis, from 1-100 applications should be hosted per account on this shared infrastructure with an average of perhaps 10-20 applications per account. With hundreds of accounts (some that are designated for development and test and some that are designated for production workloads), this model can support many thousands of applications and their development.

A good starting point for developing the standardized application hosting infrastructure is IBM Cloud's VPC landing zone deployable architecture. This IBM Cloud for Financial Services compliant solution sets up everything that is needed to host VM or Container based applications in a fully secure and compliant fashion. Also, the deployable architecture is supported and maintained by IBM. As new compliance requirements arrive or vulnerabilities are discovered, the solution is updated to maintain security and compliance.


## Separate management from workload
{: #mgmt-workload}

In the IBM Cloud Framework for Financial Services [reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about), management workloads are separated from application workloads to support the different access models, data privacy requirements, network access requirements, and so on. Because these principles apply more broadly than just in the Financial Services industry, they have been applied in this general recommendation.



### Related controls in IBM Cloud Framework for Financial Services
{: #mgmt-workload-controls}

The following [IBM Cloud Framework for Financial Services controls](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-2 Application Partitioning](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-2) |
| | [SC-3 Security Function Isolation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-3)
{: caption="Table 3. IBM Cloud Framework for Financial Services controls" caption-side="bottom"}

## Work within IBM Cloud operating limits and constraints
{: #constraints}

IBM Cloud, like all public clouds, have certain quotas, limits, and constraints on deployment patterns, scale, and so forth. This recommendation must align to and keep within all hard limits and constraints.

Examples of such quotas and limits include:

- [VPC quotas and limits](/docs/vpc?topic=vpc-quotas)
- [IAM limits](/docs/account?topic=account-known-issues#iam_limits)
- [Enterprise account limits](/docs/secure-enterprise?topic=secure-enterprise-what-is-enterprise#enterprise-hierarchy)
- [Transit Gateway limits](/docs/transit-gateway?topic=transit-gateway-helpful-tips#service-limits)
