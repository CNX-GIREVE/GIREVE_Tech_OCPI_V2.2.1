### [<- Back to module selection](cpo_edits.md)

# Contents

* [Sessions Module Specifications](#sessions-module-specifications)
  - Session Initialisation
  - Smart charging use cases
  - PATCH Sessions
  - Minimum Interval Between Session Updates
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

## Minimum Interval Between Session Updates

Gireve recommends respecting a minimum interval of two minutes between Push (PUT / PATCH) Session update requests.

If a CPO sends session updates at intervals shorter than two minutes, these requests may be considered as spam and filtered by the Gireve platform. In such cases, the updates will neither be accepted nor forwarded to the eMSP.

This recommendation applies exclusively to meter value updates (i.e., all changes except session status). Session status updates are not subject to this minimum interval constraint and may be processed without this restriction.

## Store and forward – PUT Sessions

A Store and Forward mechanism must be implemented to ensure that no session is lost in case of connection issues. Any PUT session request that does not receive a successful response (HTTP 2XX) from the Gireve IOP platform must be stored on the CPO side, with an active retry process. 

Once the connection is restored, session messages must be resent in a FIFO order. 

Retries should only be performed in two specific cases: the initial session update (status ACTIVE) and the final session update (status COMPLETED). For all intermediate updates, retries are unnecessary and not recommended, as only these key transitions carry functional and contractual value according to OCPI.

Additionally, retries must not be performed immediately when receiving platform error codes such as 425 (Too Early) or 429 (Too Many Requests), as these indicate that requests are being sent too early or too frequently; immediate retries would worsen the situation. In such cases, the client is expected to wait several minutes before retrying, using a progressive backoff strategy (e.g., 5 min → 10 min → 20 min → …). 

Retries must never be executed in an uncontrolled loop or in parallel bursts. A strict retry policy should be applied: no more than one retry every defined interval (e.g., 5 min → 10 min → 20 min → …), with ideally one processing queue per flow type (Sessions, CDRs, Tokens, etc.) and sequential handling to ensure stability and compliance.

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
