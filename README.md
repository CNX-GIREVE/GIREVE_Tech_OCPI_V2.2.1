# GIREVE OCPI V2.2.1
IOP – OCPI 2.2.1 Interface - GIREVE Implementation Guide

For more information, check the standard OCPI specifications : [OCPI 2.2.1](https://github.com/ocpi/ocpi/tree/release-2.2.1-bugfixes?tab=readme-ov-file).

- [Gireve_Implementation_Guide.pdf](https://)

- Information before starting your implementation : [Integration Guidelines](integration_guidelines.md).
- [Q&A](q&a.md)

# Contents
- [Version History](version_history.md)
- [CPO Specific implementation guidelines](cpo_edits.md).

## [Introduction](introduction.md)
*  [Aims](introduction.md#aims)
*  [Intended Audience](introduction.md#intended-audience)
*  [Definitions and Abbreviations](introduction.md#definitions-and-abbreviations)
*  [Cardinality expression](introduction.md#cardinality-expression)
  
## [Integration Guidelines](integration_guidelines.md)
* [Technical](integration_guidelines.md#technical)
  - Supported OCPI versions
  - Security
  - Platform and operator identification
  - Traceability: X-Correlation-ID and X-Request-ID
  - Multi-tenant and multi-role capability
  - IOP is a “HUB”
  - Client owned object push
  - Pagination
  - Pulling Limits
  - List of OCPI Modules
  - 
* [GIREVE management of Locations data](integration_guidelines.md#gireve-management-of-locations-data)
* [Roaming](integration_guidelines.md#roaming)
  - General workflow
  - Management of B2B tariffs
  - RFID Tokens

## [CPO Specfic Implementation Guidelines](cpo_edits.md)
* [CPO Operation Definition And Naming Rules](cpo_registration.md/#cpo-operation-definition-and-naming-rules)
* [CPO Operation And Roaming Offers](cpo_registration.md/#cpo-operation-and-roaming-offers)
  - Uses Cases Covered by IOP
  - Uses Cases Covered by Gireve  
* [Connection & Register Specifications](cpo_registration.md)

* [Locations Module Specifications](cpo_locations.md)
  - Locations informations required by Gireve
  - Static and dynamic attributes
  - "tariff_ids" property
  - "publish" property
  - Store and Forward – PUT and PATCH Locations
  
* [Tokens Module Specifications](cpo_tokens.md)
  - Download of Tokens not recommended
  - "LocationReferences" mandatory in POST Tokens Authorize requests
    
* [Commands Module specifications](cpo_commands.md)
  - List of available Commands
  - New field "connector_id" in START_SESSION
    
* [Sessions Module Specification](cpo_sessions.md)
  - Session Initialisation
  - Smart charging use cases
  - PATCH Sessions
  - Store and forward – PUT Sessions
  - Advenir specific use case
    
* [Cdrs Module Specification](cpo_cdrs.md)
  - CDR sending frequency
  - CDR content
  - CreditCDR
  - Store and forward – POST CDRs
  - Advenir specific use case
    
* [Tariffs Module Specification](cpo_tariffs.md)
  - Locations tariff update
  - Tariff shall be immutable
  - Differentiate tariff per eMSP
  - Tariffs are attached to the EVSE level
  - Store and forward – PUT Tariffs


