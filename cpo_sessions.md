### [<- Back to module selection](cpo_edits.md)

# Contents

* [Sessions Module Specifications](sessions-module-specifications)
  - Session Initialisation
  - Smart charging use cases
  - PATCH Sessions
  - Store and forward – PUT Sessions
  - Advenir specific use case

***

# `Sessions Module Specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_sessions.asciidoc)

## Session initialisation

The CPO shall initialize a Session (i.e. send a PUT Session) after an allowed authorization and when the EV plugs to the EVSE. 
This flow gives information to eMSP that the charge session of its customer has started.

## Smart charging use cases

In its current implementation of OCPI 2.2.1, Gireve has not implemented smart charging use cases. An operator is not able to send its “Charging Preferences” on an ongoing charging session.

## PATCH Sessions

Although Gireve has not implemented PATCH Sessions In its current OCPI 2.2.1 implementation, it is recommended for CPOs to send PATCH Sessions.
Connected partners won’t have to change their OCPI implementation when Gireve makes the PATCH Sessions available.

## Store and forward – PUT Sessions

A Store and Forward mechanism must be implemented to ensure that no Session may be lost, in case of a connection loss. Any PUT session that didn’t get a correct response (HTTP code: 2xx) from  Gireve IOP platform must be stored on the CPO’s side and a retry process must be active. After the connection recovery, the session messages must be resent in a FIFO manner.

## Advenir specific use case

Gireve enables CPOs to use their connection to IOP to transfer their consumption information (i.e. Sessions and CDRs) to the French subsidy program Advenir.
If used, CPOs shall send all consumption information of the charging station to IOP, even if it is not a roaming charge.
In case it is not a roaming charge, the CPO shall anonymise Sessions and CDRs with the following values :

| Attribute |	Value |
| ----------- | ----------- |
| CdrToken.country_code |	$$(*) |
| CdrToken.party_id	| ADV(*) |
| CdrToken.uid	| 12345671234567(*) |
| CdrToken.type	| RFID(*) |
| CdrToken.contract_id	| Gireve2Advenir(*) |

#### (*) Information to confirm by Gireve during onboarding

### Information and requirements

-   Unlike Locations and Tariffs where IOP forwards CPO Ids to eMSPs, Session Ids and authorization references are replaced by Gireve ones.
-   This decision has been made to ensure the consistency with other protocols and Gireve systems.
-   "PATCH Sessions" webservice has not been implemented in the Gireve current implementation. Nevertheless, it is recommended that CPOs already implement and use them.
-   Session updates shall be stored and forwarded to Gireve in case of a technical error.
-   Gireve billing feature doesn’t take into account or calculate VAT. In case costs are calculated by Gireve and included in Sessions/CDRs, VAT information is not filled.
