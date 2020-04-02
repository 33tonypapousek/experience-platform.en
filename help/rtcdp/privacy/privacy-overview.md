---
title: Privacy in Real-time Customer Data Profile
seo-title: Privacy in Real-time Customer Data Profile
description: Real-time Customer Data Profile allows you to streamline the process of keeping your data operations compliant with privacy regulations.
seo-description: Real-time Customer Data Profile allows you to streamline the process of keeping your data operations compliant with privacy regulations.
---

# Privacy in Real-time CDP

Real-time Customer Data Platform (Real-time CDP) helps marketers bring data from multiple enterprise systems together, allowing them to better identify, understand, and engage their customers. Adobe holds consumer data privacy as a fundamental design principle and provides various controls to help marketers manage the data privacy of their customers.

The majority of Real-time CDP capabilities are powered by Adobe Experience Platform. This document provides information about the various privacy enhancement technologies supported by Real-time CDP, with links to Experience Platform documentation for more information.

## Privacy Service

Adobe Experience Platform Privacy Service allows you to streamline the process of keeping your data operations compliant with privacy regulations such as the General Data Protection Regulation (GDPR) and the California Consumer Privacy Act (CCPA). Since Real-time CDP leverages Experience Platform capabilities for data collection and storage, the access and delete requests for GDPR and CCPA should be managed within Platform. See the [Privacy Service overview](../../privacy-service/home.md) document for a more detailed introduction of the service.

There are two methods for submitting individual GDPR and CCPA data subject requests for accessing and deleting customer data:

* Use the [Privacy Service UI](https://gdprui.cloud.adobe.io/) to create and monitor access and delete requests within a visual workspace. See the [Privacy Service UI tutorial](../../privacy-service/ui/overview.md) for step-by-step instructions.
* Use the [Privacy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) to manage access and delete requests with RESTful API calls. See the [Privacy Service API tutorial](../../privacy-service/api/getting-started.md) for step-by-step instructions.

<!-- (Capability will not be available for November GA) 
## Opt-out capabilities

Real-time CDP provides two types of consumer opt-out capabilities:

1. **General opt-out**: (Waiting on info)
1. **Segment-level opt-out of sale**: Opt-out of sale requests are captured using the Profile Privacy mixin (see the section on "Handling opt-out requests" in the [Real-time Customer Profile overview](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md) for more information). Using this, you can exclude users who have opted out from a segment using boolean logic ("AND NOT") in the segment predicate.
-->

## Next steps

This document provided a brief introduction to the Privacy capabilities of Real-time CDP. For more detailed information on best practices and steps for submitting access/delete requests, please refer to the [Privacy Service documentation](../../privacy-service/home.md).