### [<- Back to module selection](cpo_edits.md)


* [CPO Operation Definition And Naming Rules](#cpo-operation-definition-and-naming-rules)
* [CPO Operation And Roaming Offers](#cpo-operation-and-roaming-offers)
* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)

# `CPO operation definition and naming rules`

A CPO operation is a homogeneous group of charging points. A CPO operation is defined by its unique eMI3 operation code also called “Operator Id” which is composed by a 2 ALPHA country code plus a 3 ALPHANUMERIC Spot Operator ID.

**Example: FR*AB1**

eMI3 standard official documentation can be downloaded here : <https://emi3group.com/documents-links/>


All EVSE included in each CPO operation must have an evse_id following the eMI3 standard naming rules defined in this specific document, page 27:  

<https://emi3group.com/wp-content/uploads/sites/5/2018/12/eMI3-standard-v1.0-Part-2.pdf>

Specifically, all evse_id included into a given operation must always start with the Operator Id. 

**Example: FR*AB1*EABCDEFG*1**

#### Information and Requirements
-   It is not allowed to include in each CPO operation evse_ids starting with a different Operator-Id.
Example: A CPO “DE*CPO” having an “evse_id” not starting by “DECPO” or “DE*CPO” is not allowed 


# `CPO operation and roaming offers`

A CPO Operation is the smallest entity that can constitute a roaming offer.
Although Gireve’s technical platform (IOP) is built to manage bilateral communications between 1 operation (i.e. CPO) and another (i.e. eMSP), it is possible to aggregate several operations into a so-called network group on Gireve’s Connect Place. It allows to publish a single roaming offer that includes several operations. The main advantages of such an offer structure are:
-   Reduces the number of roaming agreements (1 agreement instead of several bilateral contracts – one per operation).
-   Allows to add or remove operations from the network without terminating the current agreement or issuing amendments, enabling new tenants to benefit from already active roaming agreements.


# `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator. In case of connection to Gireve, here is the list of use cases that a CPO can implement :

## Credentials

 <ins>**Register - FromIOP :**</ins>
 -   Get FromIOP_versions
 -   Get FromIOP_version-details
 -   Get ToIOP_versions
 -   Get ToIOP_version-details
 -   POST FromIOP_credentials

 <ins>**Register - ToIOP :**</ins>  
 -   Get FromIOP_versions
 -   Get FromIOP_version-details
 -   Get ToIOP_versions
 -   Get ToIOP_version-details
 -   POST ToIOP_credentials

<ins>**Update Credentials - ToIOP :**</ins>
 -   PUT ToIOP_credentials

<ins>**Update Credentials - FromIOP :**</ins>
 -   PUT FromIOP_credentials

<ins>**Unregister - FromIOP :**</ins>
 -   DELETE FromIOP_credentials

<ins>**Unregister - ToIOP :**</ins>
 -   DELETE ToIOP_credentials

## Locations

<ins>**Push EVCI - ToIOP :**</ins>

 -   PUT ToIOP_receiver_locations
 -   PUT ToIOP_receiver_locations-evse
 -   PUT ToIOP_receiver_locations-evse-connector

<ins>**Update EVCI - ToIOP :**</ins>
 -   PATCH ToIOP_receiver_locations
 -   PATCH ToIOP_receiver_locations-evse
 -   PATCH ToIOP_receiver_locations-evse-connector

<ins>**Pull EVCI - FromIOP :**</ins>
 -   GET FromIOP_sender_locations

<ins>**Check EVCI - ToIOP :**</ins>
 -   GET ToIOP_receiver_locations
 -   GET ToIOP_receiver_locations-evse
 -   GET ToIOP_receiver_locations-evse-connector

## Tokens exchange 

<ins>**Pull Tokens - ToIOP :**</ins>
 -   GET ToIOP_sender_tokens

<ins>**Push Tokens - FromIOP :**</ins>
 -   PUT FromIOP_receiver_tokens
 -   PATCH FromIOP_receiver_tokens

## Local Authorization (Requested by CPO)

<ins>**Real-Time Authorization - ToIOP :**</ins>
 - POST ToIOP_sender_tokens-authorize

## Remote Authorization (Requested by eMSP)

<ins>**Remote start - FromIOP :**</ins>
 - POST FromIOP_receiver_commands_START_SESSION
 - POST ToIOP_sender_commands_START_SESSION-Callback

<ins>**Remote stop - FromIOP :**</ins>
 - POST FromIOP_receiver_commands_STOP_SESSION
 - POST ToIOP_sender_commands_STOP_SESSION-Callback

## Sessions

<ins>**Push Sessions - ToIOP :**</ins>
 - PUT ToIOP_receiver_sessions

<ins>**Pull Sessions - FromIOP :**</ins>
 - GET FromIOP_sender_sessions

<ins>**Check Sessions - ToIOP :**</ins>
 - GET ToIOP_receiver_sessions

<ins>**Push Sessions - FromIOP (smart charging) :**</ins>
 - PUT FromIOP_sender_sessions-charging_preferences

## CDR

<ins>**Push CDRs - ToIOP :**</ins>
 - POST ToIOP_receiver_cdrs

<ins>**Pull CDRs - FromIOP :**</ins>
 - GET FromIOP_sender_cdrs

<ins>**Check CDRs - ToIOP :**</ins>
 - GET ToIOP_receiver_cdrs

<ins>**CreditCDRs - Any :**</ins>
 - N/A

## Tariffs

<ins>**Push Tariffs - ToIOP :**</ins>
 - PUT ToIOP_receiver_tariffs

<ins>**Pull Tariffs - FromIOP :**</ins>
 - GET FromIOP_sender_tariffs

<ins>**Delete Tariffs - ToIOP :**</ins>
 - DELETE ToIOP_receiver_tariffs

<ins>**Check Tariffs - ToIOP :**</ins>
 - GET ToIOP_receiver_tariffs

## Hub Client Info

<ins>**PUSH Hub Client Info - FromIOP :**</ins>
 - PUT FromIOP_receiver_clientinfo

<ins>**PULL Hub Client Info - FromIOP :**</ins>
 - GET ToIOP_sender_clientinfo

## Booking

<ins>**Reserve now - FromIOP :**</ins>
 - POST FromIOP_receiver_commands_RESERVE_NOW
 - POST ToIOP_sender_commands_RESERVE_NOW-Callback

<ins>**Cancel reservation - FromIOP :**</ins>
 - POST FromIOP_receiver_commands_CANCEL_RESERVATION
 - POST ToIOP_sender_commands_CANCEL_RESERVATION-Callback

## Commands

<ins>**Unlock Connector - FromIOP :**</ins>
 - POST FromIOP_receiver_commands_UNLOCK_CONNECTOR
 - POST ToIOP_sender_commands_UNLOCK_CONNECTOR-Callback

# `Use cases required by Gireve`

Some use cases are required when connecting to Gireve.

**Always required**

| Use case | Why? |
| ----------- | ----------- |
| **Register – FromIOP** **Or Register - ToIOP** | These use cases are needed to initialise connection between a platform and IOP |
| **Push EVCI - ToIOP** | A CPO connected to Gireve must transfer *“in real time”* EVSE status changes of its EVSEs and *tariff_ids* of its Connectors |
| **Pull EVCI - FromIOP** | Gireve wants to be able to refresh EVCI data when needed. |  

**If the CPO implements the “Roaming” feature**

| Use case | Why? |
| ----------- | ----------- |
| **Real-Time Authorization - ToIOP** | A CPO should be able to request eMSP through IOP when a driver uses his RFID badge or a *PnC* Contract certificate to charge. |
| **Remote start - FromIOP** | Remote authorisation and start features on CPO infrastructure are required by Gireve |
| **Remote stop - FromIOP** | Remote stop features on CPO infrastructure are required by Gireve |
| **Push Sessions - ToIOP** | A CPO must be able to send information about charging-sessions through Session objects (charge started, ...) |
| **Push CDRs - ToIOP** | The CPO must send the CDR in real time after the end of the charging-session. |


**If the CPO doesn’t commit and describe its tariffs in a roaming agreement**
| Use case | Why? |
| ----------- | ----------- |
| **Push Tariffs - ToIOP** | CPOs must inform in real-time, through IOP, eMSPs about tariff changes. |

#### Information and Requirements
-   The implementation and certification of the OCPI Tariffs module depends on the tariff’s strategy of the CPO. It is not required if the CPO describes its tariffs through the Gireve connect place. 
-   CDRs shall be sent as soon as possible after the end of the charge.
