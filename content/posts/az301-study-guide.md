---
title: "AZ-301 Study Guide"
date: 2018-03-18T02:01:58+05:30
description: ""
tags: [azure, study, certification, microsoft]
draft: true
---

# Determine Workload Requirements

[Identify Governance and Compliance](https://docs.microsoft.com/en-us/azure/architecture/framework/security/governance)

[Identifiy Identity and Access Management](https://docs.microsoft.com/en-us/azure/architecture/framework/security/identity)

[Identity Service-Oriented Architectures](): Integration Patterns
- ETL: Extract, Transform, and Load
- SSO 
- REST

[Identify Accessibility Requirements](https://en.wikipedia.org/wiki/Web_accessibility): Accessibility is about ensuring that the widest range of users, such as those with disabilities, have the same experience. This requires that you understand your target audience and know what standards will best suit your application. Some accessibility standards include:
- Web Content Accessibility Guidelines (WCAG)
- Accessible Rich Internet Applications (WAI-ARIA)
- User Agent Accessibility Guidlines (UAAG)
- Authoring Tool Accessibility Guidelines (ATAG)
- Mobile Accessibility

[Identify Availability Requirements](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/availability): High Availability is a feature of redundant or resilient system design that is usually expressed with "9s" such as 99.9% (or three nines) availability. This is a guarantee that said system will remain up for a specific amount of time. Azure Examples:
- Availability Set: Multiple VMs, Single region, single datacenter. 99.95% Availability
- Availability Zone: Multiple Vms, Single region, multiple datacenters. 99.99% Availability
- Single Instance VM: 99.9% Availability

[Identify Capacity Planning and Scalability Requirements](https://docs.microsoft.com/en-us/azure/architecture/framework/scalability/capacity): 

[Identify Deployability Requirements](https://docs.microsoft.com/en-us/azure/app-service/deploy-staging-slots): The question of deployability is a question of how quickly and cost effectively can changes be made? To know this, you need to take a few other things into account such as: What can change? When can it change? Who can change it? And what will it cost? Azure App Service Deployment Slots can help seamlessly swap between environments.

[Identify Maintainability Requirements](https://azure.microsoft.com/en-us/blog/updated-azure-business-continuity-technical-guidance/): What is required to maintain a stable system in the event of planned and unplanned outages. Including knowing when outages occur and being able to discover why.
- Azure Site Recovery
- Azure Backup
- Monitoring and Logging


[Identify Security Requirements](https://docs.microsoft.com/en-us/azure/security/fundamentals/overview): Define what areas of risk a system or application has and what authentication and authorization methods are required to protect against and mitigate attacks.


[Identify Sizing Requirements](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs): Knowing how to right-size VMs to optimize efficiency and cost. 


# Optimize Consumption Strategy

[Cost Management](https://app.pluralsight.com/library/courses/microsoft-azure-consumption-strategy-optimizing/table-of-contents)
- Not necessarily cost minimization
- Don't deliver a solution that is lower cost at the expense of business requirements
- Success is measured against business requirements
- Requirements are both technical and non-technical
- Architecture, monitoring & visibility are necessary
- In-depth Azure solution knowledge needed
- Regularly reassess as services and requirements change
- Make use of all the Azure tools available

[Optimize Storage Costs]():

[Optimize Compute Costs]():

[Optimize Network Costs]():

[Optimize App Service Costs]():

[Optimize Identity Costs]():
