---

copyright:
  years: 2023
lastupdated: "2023-06-29"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Business continuity and disaster recovery
{: #bcdr}

A holistic strategy for business continuity and disaster recovery is critical when you are managing cloud at scale. This recommendation extends the business continuity recommendations from the [IBM Cloud framework for Financial Services](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr) and provides some additional specifics that extend the {{site.data.keyword.cloud_notm}} [General disaster recovery strategy](/docs/overview?topic=overview-understanding-dr#bcdr-general).

Key points from the IBM Cloud Framework for Financial Services:

- Establish backup storage in a geographically separate region from the workload that is backed up
- Conduct regular and frequent backups of all information necessary to restore operation of the system
- Monitor backups and test recovery procedures to ensure success if recovery is required
- Protect the confidentiality, integrity, and availability of backup information at storage locations
- Establish contingency plans that include procedures for validating successful recovery and reconstitution of the original configuration

## High availability deployments
{: #ha}

For [high availability deployments](/docs/overview?topic=overview-understanding-dr#dr-categories), additional copies of infrastructure and applications should be deployed in the same workload account and by the same projects as the primary infrastructure. This makes it easier to ensure that additional regions are kept in sync and simplifies configuring access for DevOps teams.

### Infrastructure as Code
{: #ha-iac}

Infrastructure as code should be used to create any additional deployments for high availability, whether it be Active/Active, Active/Standby, or Active/Passive. This helps ensure that additional regions that are created for high availability are kept in sync with the primary regions and provides all the other security, compliance, and governance benefits of using infrastructure as code.

### Active/Passive
{: #active-passive}

When using an active/passive (cold standby) strategy, infrastructure as code enables contingency plans that include use of automation to create infrastructure on demand. This allows for very inexpensive active/passive configurations as costly infrastructure does not need to be maintained. Given the low cost of this approach, an Active/Active or Active/Standby high availability strategy can be combined with Active/Passive, particularly for important workloads.

Ensure that automation code such as terraform, deployable architectures, and associated configurations are included in the data that is backed up and made highly available.
{: note}

#### Preparation for Active/Passive
{: #prep}

There are [tradeoffs](/docs/overview?topic=overview-understanding-dr#dr-categories) to be made between speed of recovery and cost. As setting up physical network connectivity is a slow process, direct-link connectivity to at least one alternative region should be established as a minimum even for Active/Passive (cold standby) configurations.

After network connectivity is available, the recovery strategy can be tailored for each BU or each workload. At the low cost, slow recovery end of the spectrum, all required infrastructure in the alternative region is re-created from infrastructure as code. At the other end of the spectrum, a copy of all infrastructure is maintained with running applications to allow near instant recovery. In between are options where bootstrap infrastructure is maintained, such as transit gateways and schematics agents to reduce recovery time. In all cases, after the infrastructure is available, data is then restored from backup before activating the passive deployment.

#### Activating the alternative region
{: #alternate-region}

Should a regional outage occur, an alternative region can be activated following pre-established and tested procedures. These procedures can include deploying infrastructure in the region by using project configurations that were created and tested in advance so that error free deployments can readily be accomplished. If pre-existing projects are not maintained, project configurations can be restored from backup and modified to target the alternative region.

Platform services such as enterprise accounts, accounts, IAM, and catalog are global and thus are not affected by regional outages. The enterprise account hierarchy, private catalogs, and all IAM configuration can continue to be used unchanged during a disaster recovery process. Regional service instances that include networking, VPC, various compute, storage, and databases must be recovered in the new region.


## Backups
{: #backups}

Backups must be stored in a different region from the workload, and ideally in a separate backup account. This separation in location and management domain (account) minimizes the chances that an outage, operational error, or security breach that affect the workload will also affect the backup. Backup regions should be multi-zone IBM Cloud regions or highly available Satellite regions. However, each family of services on IBM Cloud has its own backup mechanism that introduces some limitations that impact this overall strategy. For more information, see [service considerations](#service-considerations).

In addition, a "disaster recovery kit" should be developed and stored outside of IBM Cloud. The disaster recovery kit is an off-site and protected repository that includes hardware, software, and system secure configurations, one-time keys, and the disaster recovery plan. Use this disaster recovery kit during an incident response to restore services.

### Infrastructure as code
{: #backup-iac}

Infrastructure as code ensures that good backup practices are implemented in a uniform way across the organization. Include implementation of backups as part of deployable architectures. Keep backup configuration close to provisioning code so that it is easier to ensure that backups are adjusted as information system changes occur. Create and configure backup accounts as code.

### Backup accounts
{: #accounts}

Each BU account group should include a separate backup account and an additional backup account should be used for the centralized accounts. These backup accounts provide an extra layer of protection for the confidentiality and integrity of the backed up data. If credentials to a source account are compromised, it should not be possible to use that access to tamper with backups.

Supporting this effort, authorization to backup data in the backup account should use service-to-service or service to trusted profile authorization where possible. Otherwise, use secure storage of credentials with least privilege access.

If you are using Cloud Object Storage for backup, at minimum, use separate buckets for backing up different workload accounts. Also, use separate backup credentials for each bucket where service to service policies are not possible. Use Cloud Object Storage versioning or [immutable buckets](/docs/cloud-object-storage?topic=cloud-object-storage-immutable) for maximum security.

Services that provide built in backup facilities might have limitations regarding backup accounts.
{: note}

### Service-specific considerations
{: #service-considerations}

#### IBM Cloud Databases
{: #icd}

[IBM Cloud Databases](/docs/cloud-databases?topic=cloud-databases-about) provide automatic daily backups that are stored in the same account and geography. Backups are typically stored in [cross-regional storage](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#backup-locations) and should therefore be immune to a single region outage. This facility is reliable and convenient, but does not currently support backup storage in a specified alternate region or account. However, backups can be [restored to another region or account](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui), so recovery to a passive deployment (cold standby) is supported.

It is also possible to use database-specific clients (for example pg_dump for PostgreSQL) to backup IBM Cloud Databases to arbitrary storage by using customer automation. If used, these backups should be stored in Cloud Object Storage in an alterative region and in the backup account as described in [Backup accounts](#accounts).

#### Virtual servers
{: #vsi}

Both [volume snapshots](https://cloud.ibm.com/docs/vpc?topic=vpc-backups-vpc-best-practices&interface=ui) and [Veeam](/docs/vpc?topic=vpc-about-veeam) are available to backup virtual servers. Veeam is preferred for extensive feature set and its ability to backup application workloads. However, Veeam does require deploying an agent to backup workloads not hosted on VMWare. The Veeam server that is used for backups should be located in a alterative region and in the backup account as described in [Backup accounts](#accounts).

#### Red Hat OpenShift / Kubernetes
{: #roks-kub}

Red Hat OpenShift and Kubernetes clusters on IBM Cloud can be backed up through [PX-Backup](/docs/containers?topic=containers-storage_portworx_backup). The Portworks server that is used for backups should be located in a alterative region and in the backup account as described in [Backup accounts](#accounts).

#### Secrets Manager
{: #secrets-mgr}

Secrets manager can be [backed up using the IBM Cloud CLI](/docs/secrets-manager?topic=secrets-manager-ha-dr#auto-backup) and customer-supplied automation. These backups should be stored in a backup instance of secrets manager in an alterative region and in the backup account as described in [Backup accounts](#accounts).

#### Cloud Object Storage
{: #cos}

Cloud Object Storage buckets can be cross regional, regional, or stored in a single data center. In addition, Cloud Object Storage Buckets can be backed up using [bucket replication](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-replication-overview). These backups should be stored in a Cloud Object Storage bucket in a alterative region and in the backup account as described in [Backup accounts](#accounts).


#### Cloudant
{: #cloudant}

Cloudant is a highly available NoSQL DB that can be configured with replicas in multiple regions. In addition, Cloudant backups can be taken by using the [CouchBackup](/docs/Cloudant?topic=Cloudant-ibm-cloudant-backup-and-recovery) client that uses customer-supplied automation. These backups should be stored in Cloud Object Storage in a alterative region and in the backup account as described in [Backup accounts](#accounts).

#### Event Streams
{: #event-streams}

Event Streams is a highly available Kafka as a service. Given the nature of eventing systems, point in time backups are likely of little value. However, [mirroring to a second region](/docs/EventStreams?topic=EventStreams-mirroring_setup) is recommended for disaster recovery.

#### Other services
{: #other-services}

For more information on backups, see service-specific documentation.


### Related controls in IBM Cloud Framework for Financial Services
{: #iac-controls}

The following [IBM Cloud Framework for Financial Services controls](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure that you meet the requirements.

| Family | Control |
|--------|---------|
| Contingency Planning (CP) | [CP-2 Contingency Plan](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2)|
| | [CP-6 Alternate Storage Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-6) |
| | [CP-7 Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) |
| | [CP-9 Information System Backup](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-9) |
| | [CP-10 Information System Recovery and Reconstitution](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10) |
{: caption="Table 1. IBM Cloud Framework for Financial Services controls" caption-side="bottom"}
