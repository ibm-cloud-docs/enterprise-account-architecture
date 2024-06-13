---

copyright:
  years: 2023, 2024
lastupdated: "2024-04-26"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Central administration account
{: #admin-hub-account}

This administrative account centralizes the management of the deployable architectures that are needed to provision the enterprise account structure, the Security and Compliance Center, and the tools account across both development and production.

![Central administration diagram. All of the information is conveyed in the surrounding text.](images/admin-hub.svg){: caption="Figure 1. Central administration account" caption-side="bottom"}

The central administration account contains a catalog of deployable architectures that are deployed through IBM Cloud projects to manage the top-level accounts in both development and production to ensure that both enterprises have nearly the same configuration and to provide centralized management. This account also holds the schematics agent to enable custom deployable architectures stored in corporate Git.

| Component | Quantity | Description |
|-----------|--------------|----|
| Security and Compliance project | 1 |  Manages the infrastructure as code for deploying the Security and Compliance Center and its dependencies in this account. |
| Network and Services project | 1 | Manages the infrastructure as code for deploying centralized networking and other common services into the central administration account. |
| Business unit project | 1-25 | Manages the infrastructure as code for deploying a business unit account group, a backup account, the business unit administration account, and the BU administration account contents. |
| Central administration project | 1 | Manages the infrastructure as code for deploying the resources in the central administration account, creating and configuring the administration backup account. |
| Private catalog | 1 | Used to host the approved deployable architectures for the projects in this account. |
| Schematics agent | 1 | Used enable privately hosted custom deployable architectures in the private catalog for this account. |
| Security and Compliance Center | 1 | Monitors enterprise root account and the administration account group in both production and nonproduction enterprises |
{: caption="Table 1. Components" caption-side="bottom"}

The schematics agent, catalog, projects, and schematics workspaces must co-reside in a single account.
{: important}

Additional components not shown in the diagram:

| Component | Quantity | Description |
|-----------|--------------|----|
| Activity Tracker | 1 | Provides an audit trail for activity within the account |
| IBM Cloud Logging | 1 | Provides log monitoring for the infrastructure hosting the Schematics Agent |
| IBM Cloud Monitoring | 1 | Provides performance and error monitoring for the Schematics Agent |
| Event Notifications | 1 | Provides notifications for projects and the Security and Compliance Center |
| Cloud Object Storage | 1 | Used to store Security and Compliance Center results |
| Compliance access group | 1 | Used to authorize users view scan results |
{: caption="Table 2. Additional components" caption-side="bottom"}

## Infrastructure as code
{: #iac}

The central administration account hosts the infrastructure as code and configuration to perform the initial setup and ongoing maintenance of the key accounts in the enterprise architecture.

![Accounts that are managed from the central administration account. All of the information is conveyed in the surrounding text.](images/central-admin-iac.svg){: caption="Figure 2. Accounts managed from the central administration account" caption-side="bottom"}



## Authorization
{: #admin-auth}

Rather than authorizing users to deploy infrastructure as code directly, authorize project instances within the central administration account to deploy resources and create child accounts. Authorizing projects to deploy resources and create child accounts ensures that only changes to the enterprise account structure and key shared infrastructure can be made through projects. As projects are subject to the governance provided by catalog onboarding and project configuration management, this helps implement the zero trust security best practice that is required by many compliance programs, including [Financial Services Cloud](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-zero-trust).

User access to the projects and resources within the Central Administration account is best performed with Access Groups or Trusted Profiles. This keeps the number of policies low, making it easier to reason about access permissions and reducing the opportunity for error. A typical organization should be able to operate their central administration account with on the order of 10 access groups or trusted profiles.

## Rationale for centralized management across development and production
{: #rationale-for-central-proj}

Centralizing management of deployable architectures and their configuration into an admin account provides the following benefits:

- Single place to find IaC related to enterprise administration. The admin catalog ensures that only approved, tested, and compliant deployable architectures are available to the user. Using a single catalog allows administrators to find and use these deployable architectures easily.
- One catalog to publish IaC related to enterprise administration. Having a single catalog to maintain makes it easier to ensure that the correct set of deployable architectures is exposed and maintained. This includes ensuring that updates to deployable architectures are available and deployed in a timely fashion.
- Centralized access control and monitoring for key Enterprise infrastructure. Placing the catalogs and projects in a centralized account means that only a single set of access policies must be audited to ensure the correct (limited) set of users have access to manipulate this infrastructure.
- Use of projects with trusted profiles also ensures that credentials with the capability to manipulate this fundamental configuration are not directly accessible to users and thus cannot be misused.  Projects also ensure configuration governance, making it impossible for a single individual to modify key infrastructure.
- Only one schematics agent is needed.


## Bootstrapping the central administration account
{: #bootstrap-central-admin-account}

As the central administration account is responsible for laying down the majority of the enterprise account architecture, the enterprise accounts with IBM Cloud and the Central Administration account must be created first.

Steps to bootstrap the Central Administration Account:

1. Manually create a development and production enterprise based on two freshly created subscription accounts ([docs](/docs/secure-enterprise?topic=secure-enterprise-create-enterprise&interface=ui)).
2. Manually create a child account for centralized administration in the production enterprise ([docs](/docs/secure-enterprise?topic=secure-enterprise-enterprise-add&interface=ui#create-accounts))
3. Onboard deployable architectures for the network and services account and for the central administration account to a private catalog ([docs](/docs/secure-enterprise?topic=secure-enterprise-manifest)). The deployable architecture source can be stored in a private Git repo accessible over the public network during the bootstrap procedure.
4. Deploy the network and services account through a project ([docs](/docs/secure-enterprise?topic=secure-enterprise-setup-project)). This includes setting up direct link to provide connectivity to on-premises networking.
5. Populate the central administration account through a project. This includes provisioning the schematics agent to enable access to private Git repos stored on premises (required if these private Git repos are not accessible from IBM Cloud through the public internet).
6. (Optional) Update the private catalog entries for previously onboarded deployable architectures to point to a Git repo on the private network for future maintenance.
