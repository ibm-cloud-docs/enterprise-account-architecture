---

copyright:
  years: 2023
lastupdated: "2023-03-29"

subcollection: enterprise-account-architecture

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Large enterprise requirements
{: #account-reqs}

Large enterprises often have similar requirements for scale, security infrastructure, and governance.
{: shortdesc}

The following table includes some of these common requirements that are targeted by the architecture that is covered in this white paper.

| Domain | Requirement | Detail |
| ----------- | ----------- | ----------- |
| Resource Scalability | Compute | Up to 40k hosts, 160k vCPUs |
| | Containers | Up to 20k Containers |
| | Storage | Up to 50 PB (main) & 30 PB (backup) |
| | Applications | Up to 5,000 apps |
| | Growth | Support 20% Y/Y growth |
| Organization Scalability | Business Units | Up to 25 |
| | Portfolios | Up to 250 |
| | Users | Up to 100k |
| Security | Network security and enterprise integration | Multi-vendor VNF integration
| | Segmentation and isolation | Zero trust: deny first, allow what you must |
| | Onboarding, offboarding, or consolidation of units and teams | |
| | Network intelligence | Enterprise scale, virtualized firewalls |
| | Data protection | Encryption, BYOK, audit, exfil prevention |
| Infrastructure | Common services that are shared across teams and accounts| |
| |Common network connectivity to on-premises | |
| |Centrally managed enterprise transit| |
| |Unified usage reporting and billing| |
| |Quotas by unit, team, apps| |
| | Application migration and restructuring | |
| |Shared logging facility| |
| Governance | Templated deployment support | Automated deployment of approved infra patterns |
| | Enterprise policy | |
| | Role inheritance across enterprise| |
| | Maximize cost effectiveness| |
| | Integration with governance tools| Includes security advisor |
| Compliance | Financial Services Cloud | Compliant by default, audited, shift left |
| | SOC2, GDPR, and so on | Compliance coverage |
{: caption="Table 1. Common Requirements for large enterprises" caption-side="bottom"}
