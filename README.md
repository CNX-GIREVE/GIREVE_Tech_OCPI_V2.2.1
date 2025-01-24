# GIREVE OCPI V2.2.1
IOP – OCPI 2.2.1 Interface - GIREVE Implementation Guide

For more information, check the standard OCPI specifications : [OCPI 2.2.1](https://github.com/ocpi/ocpi/tree/release-2.2.1-bugfixes?tab=readme-ov-file).

- [Gireve_Implementation_Guide.pdf](https://)

- Information before starting your implementation : [Integration Guidelines](integration_guidelines.md).
- [Q&A](q&a.md)

# Contents
- [Version History](version_history.md)
- [CPO Specific implementation guidelines](cpo_edits.md)
- [eMSP Specific implementation guidelines](emsp_edits.md)

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
  - Gireve management of Ids
    
* [GIREVE management of Locations data](integration_guidelines.md#gireve-management-of-locations-data)
* [Roaming](integration_guidelines.md#roaming)
  - General workflow
  - Management of B2B tariffs
  - RFID Tokens

## [CPO Specfic Implementation Guidelines](cpo_edits.md)
* [CPO Operation Definition And Naming Rules](cpo_registration.md/#cpo-operation-definition-and-naming-rules)
* [CPO Operation And Roaming Offers](cpo_registration.md/#cpo-operation-and-roaming-offers)
* [Uses Cases Covered by IOP](cpo-emsp_covered_by_gireve.md)
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
 
## [eMSP Specfic Implementation Guidelines](emsp_edits.md)
* [Uses Cases Covered by IOP](cpo-emsp_covered_by_gireve.md)
  - Uses Cases Covered by Gireve  
* [Connection & Register Specifications](cpo_registration.md)

* [Locations Module Specifications](emsp_locations.md)
  - Static and dynamic attributes
  - PULL Locations: Retrieve Locations of a single given CPO
  - PULL Locations ToIOP: Get List, Full and Delta modes
  - PULL Locations ToIOP: evse_id
  - PULL Locations ToIOP: “gireve_id” An Extra Gireve Property
  - PULL Locations ToIOP: tariff_ids
  - PULL Locations ToIOP: Plug&Charge (P&C)
  - PULL Locations ToIOP: Connector Standards
  - Fields Not Implemented by Gireve
  - PUSH Locations FromIOP

 * [Tokens Module Specifications](emsp_tokens.md)
  - Push Tokens ToIOP
  - PULL Tokens FromIOP
    
* [Commands Module specifications](emsp_commands.md)
  - “ocpi-to-country-code” and “ocpi-to-party-id” headers mandatory in StartSession and StopSession commands
  - “evse_uid” mandatory in StartSession command
  - “connector_id” optional in StartSession command
  - ReserveNow command
  - CancelReservation command
  - UnlockConnector command

* [Sessions Module Specification](emsp_sessions.md)
  - Session: Object IDs
  - Session: ‘VAT’
  - Session: ‘SmartCharging’
  - PULL Sessions ToIOP: Get List Pagination
    
* [Cdrs Module Specification](emsp_cdrs.md)
  - CDR: Object IDs
  - CDR content
  - Add billing information in “Remark” field
  - Fields Not Implemented by Gireve
  - PULL CDRs ToIOP: Get List Pagination 
    
* [Tariffs Module Specification](emsp_tariffs.md)
  - PULL Tariffs ToIOP: Object ID
  - PULL Tariffs ToIOP: “gireve_id” An Extra Gireve Property
  - PULL Tariffs ToIOP: Tariff Type
  - PULL Tariffs ToIOP: CPO Targeting
  - PULL Tariffs ToIOP: eMSP Targeting
  - PULL Tariffs ToIOP: Get List Pagination
  - Specific properties added by Gireve
