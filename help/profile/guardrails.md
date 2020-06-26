---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: Experience Platform guidelines
topic: guide
---

# Platform guardrails for Real-time Customer Profile

Real-time Customer Profile provides centrally accessible individual profiles that allow you to target your customers with personalized cross-channel experiences based on behavioral insights and customer attributes. In order to achieve this targeting, Profile and the segmentation engine within Platform use a highly denormalized hybrid data model. The hybrid data model offers a new approach to developing customer profiles making it important for the data to be modeled correctly. While the Profile Store maintaining profile data is not a relational store, Profile permits integration with small dimension entities in order to create segments in a simplified and intuitive manner. This integration is known as multi-entity segmentation. 

Adobe Experience Platform provides a series of guardrails to help you avoid creating data models which Real-time Customer Profile cannot support. This document outlines the best practices and constraints when using dimension entities, specifically in batch segmentation. 

## Getting started

It is recommended that you read the following Experience Platform services documentation before attempting to build data models for use in Real-time Customer Profile. Working with data models, and the guardrails outlined in this document, require an understanding of the various Experience Platform services involved with managing Real-time Customer Profile entities: 

* [Real-time Customer Profile](home.md): Provides a unified, real-time consumer profile based on aggregated data from multiple sources.
* [Adobe Experience Platform Identity Service](../identity-service/home.md): Supports the creation of a "single view of the customer" by ridging identities from disparate data sources as they are ingested into Platform.
* [Experience Data Model (XDM)](../xdm/home.md): The standardized framework by which Platform organizes customer experience data.
  * [Basics of schema composition](../xdm/schema/composition.md): An introduction to schemas and data modeling within Experience Platform.
* [Segmentation Service](../segmentation/home.md): The segmentation engine within Platform used to create audience segments from your customer profiles based on customer behaviors and attributes.
  * [Multi-entity segmentation](../segmentation/multi-entity-segmentation.md): A guide to creating segments that integrate dimension entities with profile data.

## Entity types

The Profile Store data model consists of two core entity types:

* **Primary entity:** A primary entity, or Profile entity, merges data together to form a "single source of truth" for an individual. This unified data is represented using what is known as a "union view". A union view aggregates the fields of all schemas that implement the same class into a single union schema. The union schema for Real-time Customer Profile is a denormalized hybrid data model that acts as a container for all profile attributes and behavioral events. 

  Time-independent attributes, also known as "record data" are modeled using XDM Individual Profile, while time-series data, also known as "event data" is modeled using XDM ExperienceEvent. As record and time-series data is ingested and managed by Catalog, it triggers Real-time Customer Profile to begin ingesting data that has been enabled for its use. The more interactions and details that are ingested, the more robust individual profiles become.

  ![](images/guardrails/profile-entity.png) 

* **Dimension entity:** Your organization may also define XDM classes to describe things other than individuals, such as stores, products, or properties. These non-people schemas are known as "dimension entities" and do not contain time-series data. Dimension entities provide lookup data which aids and simplifies multi-entity segment definitions and must be small enough that the segmentation engine can load the entire data set into memory for optimal processing (fast point lookup).

  ![](images/guardrails/profile-and-dimension-entities.png)

## Limit types

When defining your data model, it is recommended to stay within the provided guardrails to ensure proper performance and avoid system errors. The guardrails provided in this document include two limit types:

* **Soft limit:** A soft limit provides a recommended maximum for optimal system performance. It is possible to go beyond a soft limit without breaking the system or receiving error messages, however going beyond a soft limit will result in performance degradation. It is recommended to stay within the soft limit to avoid decreases in overall performance.

* **Hard limit:** A hard limit provides an absolute maximum for the system. Going beyond a hard limit will result in breakages and errors, preventing the system from functioning as expected.

## Data model guardrails

Adhering to the following guardrails is recommended when creating a data model for use with Real-time Customer Profile.

### Primary entity Guardrails

| Guardrail | Limit | Limit Type | Description |
| --- | --- | --- | --- |
| Number of datasets permitted to contribute to the profile union schema | 20 |  | **A maximum of 20 Profile-enabled datasets are permitted.** To enable another dataset for Profile, an existing dataset must first be removed or disabled.|
| Number of multi-entity relationships permitted| 5 |  | **A maximum of 5 multi-entity relationships can be defined between primary entities and dimension entities.** Additional relationship mappings should not be made until an existing relationship is removed or disabled. | 
| Maximum JSON depth for ID field used in multi-entity relationship| 4 | | **The maximum JSON depth for an ID field used in multi-entity relationships is 4.** This means that in a highly-nested schema, the relationship selector is disabled for fields that are nested more than 4 levels deep. |

### Dimension entity Guardrails

| Guardrail | Limit | Limit Type | Description |
| --- | --- | --- | --- |
| No time-series data permitted in profile for non-people entities| 0 | | **Time-series data is not permitted in profile for non-people entities.** If a time-series dataset is associated with a non-people ID, the dataset cannot be enabled for Profile. |
| No nested relationships permitted | 0 | | **You cannot create a relationship between two non-people schemas.** The ability to create relationships is not supported for any schemas which are not part of the Profile union schema.|
| Maximum JSON depth for primary ID field | 4 | | **The maximum JSON depth for the primary ID field is 4.** This means that in a highly-nested schema, you cannot select a field as a primary ID if it is nested more than 4 levels deep. A field that is on the 4th nested level can be used as a primary ID.|

## Data size guardrails

The following guardrails refer to data size and are recommended to ensure data can be ingested, stored, and queried as intended. Data size will be measured as uncompressed data in JSON at time of ingestion.

### Primary entity guardrails

| Guardrail | Limit| Limit Type | Description|
| --- | --- | --- | --- |
| Maximum size per profile fragment | 10KB | Soft | **The recommended maximum size of a profile fragment is 10kB.** Ingesting larger profile fragments will affect system performance. For example, loading a heavy CRM dataset where some profile fragments are 50kB in size will result in degraded system performance.|
| Absolute maximum size per profile fragment | 1MB | Hard | **The absolute maximum size of a profile fragment is 1MB.** Ingestion will fail when attempting to upload a profile fragment that is larger than 1MB.|

### Dimension entity guardrails

| Guardrail | Limit | Limit Type | Description|
| --- | --- | --- | --- |
| Maximum total size for dimension entity (total for all records in an entity) | 200MB | | **The maximum total size for all records in a non-person entity of a given type is 200MB.** Ingesting larger dimension entities will result in degraded system performance. For example, attempting to load a 10GB product catalog as a dimension entity is not recommended.|
| Datasets per dimensional entity schema | 1 | | **A maximum of 1 dataset can be associated with each dimensional entity schema.** For example, if you create a schema for "products" and add a contributing dataset, you are unable to create a second dataset tied to the products schema.|