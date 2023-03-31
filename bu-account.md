---

copyright:
  years: 2023
lastupdated: "2023-03-29"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Business unit administration account
{: #bu-admin-account}

Each production business unit (BU) account group has a business unit administration account. This account enables BUs to self-administer their workload accounts and applications across development and production enterprises.
{: shortdesc}

Self-administration is constrained to the capabilities of the deployable architectures (infrastructure as code templates) provided in the application and infrastructure catalogs.

![BU Admin IaC diagram. All of the information is conveyed in the surrounding text.](images/bu-hub.svg){: caption="Figure 1. BU administration account infrastructure as code" caption-side="bottom"}

Deployable architectures in the infrastructure catalog enable workload accounts to be created and provisioned with the shared infrastructure needed to host applications in a secure and compliant manner.

![BU Admin service diagram. All of the information is conveyed in the surrounding text.](images/bu-hub-2.svg){: caption="Figure 2. BU administration account services" caption-side="bottom"}

Supporting the infrastructure as code elements in the BU administration account are Schematics workspaces and the Schematics agent. Schematics enables the use of deployable architectures that are stored in private Git repos that are hosted on the corporate network. This account also hosts the management and edge VPCs used in the VPC landing zone reference architecture if required by the BU.

| Component | Quantity | Description |
|-----------|--------------|----|
| App development project | n | Manages the infrastructure as code for deploying application development resources such as Git repos, toolchains, container registries, evidence lockers. The applications themselves are built and then deployed onto shared infrastructure by using these tools |
| Shared infrastructure project | n | Manages the infrastructure as code for deploying workload accounts and the shared infrastructure within those accounts |
| App catalog | 1 | Used to host the approved deployable architectures for the app projects in this account |
| Infrastructure catalog | 1 | Used to host the approved deployable architectures for the shared infrastructures projects in this account |
| Schematics agent | 1 | Used to enable privately hosted custom deployable architectures in the private catalog |
| Schematics workspaces | n | Orchestrated by projects, used to deploy the deployable architectures, and store the terraform state. One workspace per configuration within each project. |
| Management/Edge VPC | 1 | Hosts the shared management and edge resources for the business unit. This can include public load balancers, bastion hosts, and custom management services. |
{: caption="Table 1. Components" caption-side="bottom"}


## Authorization
{: #authorization}

Rather than authorizing users to deploy infrastructure as code directly, authorize [IBM Cloud project](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects) instances within the business unit administration account to deploy resources and create child accounts. Authorizing projects to deploy only resources and create child accounts ensures that changes to the business unit infrastructure can be made only through IBM Cloud projects. These changes are thus subject to the governance provided by catalog onboarding and project configuration management. This helps implement the zero trust security best practices that are required by many compliance programs, including [Financial Services Cloud](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-zero-trust).

## Rationale for centralized infrastructure as code management
{: #rational-central-iac}

Centralizing management of deployable architectures and their configuration into a production administration account for each BU provides the following benefits:

- BU operators can manage their own workloads within the constraints that are imposed by the centralized organization

   The application and infrastructure catalogs ensure that only approved, tested, and compliant deployable architectures are available. Using two catalogs makes it easy to set an IAM policy such that users have access to the correct set of deployable architectures according to their roles. For example, DevOps users get access to infrastructure and application developers get access to application development tools.

- Centralized access control and monitoring for the BU

   Placing the catalogs and projects in a centralized account makes it easier to ensure the principle of least privilege is applied. Use of projects also ensures that credentials with the capability to manipulate applications and infrastructure are not accessible to users and thus cannot be misused. Finally, keeping these related projects in the BU account makes it easy to monitor deployments and ensure that the infrastructure is up to date and compliant.

- Ensures that development and production are aligned

   Using a single project across development and production makes it possible to align development and test environments with production environments. The single project helps reduce the chance of defects that are related to environmental differences while providing control to the team over testing new deployable architecture versions in different environments. It also ensures that the lifecycle of these resources is properly managed across all environments. For example, if a project is no longer needed, it is easy to clean up all resources across nonproduction and production environments.

- Allows all project resources to be tracked for accounting and configuration management

   Using a single project across nonproduction and production ensures that all project (or application) resources are tracked and allocated to the project. Projects enforce resource tagging and track resource providence, approvals, and so on. This ensures that accounting and configuration management needs are covered.

- Only one schematics agent is needed per BU
