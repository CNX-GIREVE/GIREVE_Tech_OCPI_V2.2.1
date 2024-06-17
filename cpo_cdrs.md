### [<- Back to module selection](cpo_edits.md)

# Contents

* [Cdrs Module Specifications](#cdrs-module-specifications)
  - CDR sending frequency
  - CDR content
  - CreditCDR
  - Store and forward – POST CDRs
  - Advenir specific use case
 
  ***

# `Cdrs Module Specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_cdrs.asciidoc)

## CDR sending frequency
Gireve requires CDRs to be sent directly at the end of the charging-session.
eMSPs will therefore be able to display the charging-session price directly to their end customers.

## CDR content
The “total_time” value is the total duration of this session (including the duration of charging and not charging). 
It doesn’t include the duration during which the EVSE is out of order so cannot supply the service. The out of order duration should be free of charge for eMSPs.

## CreditCDR
Although Gireve has not implemented Credit CDRs use cases in its current OCPI 2.2.1 implementation, it is recommended for CPOs to manage and send them.
Connected partners won’t have to change their OCPI implementation when Gireve makes this use case available.


## Store and forward – POST CDRs
Similarly to a PUT sessions, Store and Forward mechanism must be implemented to ensure that no CDR can be lost, in case of a connection loss. Any POST Cdrs that didn’t get a correct response (i.e. HTTP code: 2xx) from the Gireve platform IOP must be stored on CPO side and a retry process must be active. After the connection recovery, the Cdr messages must be resent in a FIFO manner.

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
