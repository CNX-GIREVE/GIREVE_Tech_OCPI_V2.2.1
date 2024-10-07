### [<- Back to module selection](cpo_edits.md)

# Contents

* [Tariffs Module Specifications](#tariffs-module-specifications)
  - Locations tariff update
  - Tariff shall be immutable
  - Tariffs Information required by Gireve
  - Differentiate tariff per eMSP
  - Tariffs are attached to the EVSE level
  - Store and forward – PUT Tariffs
 
***

# `Tariffs Module Specifications`

IOP follows the OCPI standard for Tariffs upload by a CPO. [See OCPI specifications.](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_tariffs.md)

## Locations tariff update 
In case of tariff changes on a Location, Gireve suggests  CPOs :

-   Create a new Tariff with “start_date” at the date of the change.
-   Update the "end_date" of the ongoing tariff.
-   Attach the connectors to both tariffs, current one and next one.

## Tariff shall be immutable

Management of tariffs is complex for CPOs and eMSPs, especially when tariff description changes (i.e. same tariff_id, different description). That’s why Gireve suggests CPOs  never change tariff properties attached to the price calculation. 
The only information that can change shall be textual properties or end_date.

## Tariffs Information required by Gireve

Gireve requires some information to be filled by CPOs in Tariff data, even if they are optional in the OCPI 2.2.1 standard. This information is used to ensure data quality.

*List of the required properties :*
| Property |	Reason |
| ----------- | ----------- |
| start_date-time | The CPO must send us a start_date_time to indicate the date from which the tariff applies. the tariff start_date_time is used to bill sessions with tariffs that apply when the session is active. If the CPO does not send us a start_date_time, the date of reception will be the start of the tariff's applicability. |

*Exemple :* 
- There was a bug on 09/25 in the CPO system, so we didn't receive the tariffs applicable on 09/25, nor the sessions.
- The CPO returned the same tariffs on 26/09 without start_date_time, so we set start_date_time to 26/09.
- The charge sessions of 25/09 cannot be billed with these tariffs as they are applicable from 26/09.


## Differentiate tariff per eMSP

For some reason, a CPO might want to define different tariffs for a unique Location depending on the eMSP.
Example: For eMSP A on connector X, the B2B Regular tariff is 1€/h. For all other eMSPs, the tariff is 1,20€/h.
The connection to a Hub is not compatible with this use case, that’s why Gireve has extended the Tariff object in OCPI 2.2.1 with 2 optional properties :

| Attribute |	Card |	Description |
| ----------- | ----------- | ----------- |
| target_operator_country_code | ? |	Country code of the eMSP this tariff applies to. Shall be filled if target_operator_party_id is filled. |
| target_operator_party_id	| ?	| Party Id of the eMSP this tariff applies to. Shall be filled if target_operator_country_code is filled. |

This behaviour works with the following main principles :

-   "tariff_ids" attached to a connector are the same, whatever the eMSP.
-   For a single "tariff_id", the description of the tariff (OCPI 2.2.1 Tariffs module) can be different according to the eMSP.
-   If "target_operator_country_code” and "target_operator_party_id" are null, the tariff description applies to all eMSPs (i.e. default tariff).
-   When a CPO is pulled by IOP to get tariffs, it shall respond with multiple iterations for a single "tariff_id", one for each tariff description.

Example:
*CPO FR*CPO pushes 3 times tariff "tariff_A" :*

 |tariff id |	Description | target_operator_country_code and target_operator_party_id values |
| ----------- | ----------- | ----------- |
| tariff_A	| 0,50€/h	| DE MP1| 
| tariff_A |	0,10€/kWh	| NL MP2 |
| tariff_A |	0,80€/h |	- |

*"tariff_A" is 0,80€/h for all eMSPs*

*Except for eMSP NLMP2 for which “tariff_A” is 0,10€/kWh*

*And for eMSP DEMP1 for which “tariff_A” is 0,50€/h*

## Tariffs are attached to the EVSE level
In Gireve’s model, the tariff is attached to the EVSE level and not to the connector level. IOP keeps “tariff_ids” attached to the first connector of the EVSE in the payload and applies them to the EVSE. “tariff_ids” of other connectors are ignored.


## Store and forward – PUT Tariffs
Similarly to a POST Cdrs, Store and Forward mechanism must be implemented to ensure that no Tariffs can be lost, in case of a connection loss. Any PUT Tariffs that didn’t get a correct response (HTTP code: 2xx) from  Gireve IOP platform must be stored on CPO side and a retry process must be active. After the connection recovery, the Tariffs messages must be resent in a FIFO manner. eMSP Specific Implementation Guidelines.


Information and requirements

-   In Gireve’s model, the tariff is attached to the EVSE level and not to the connector level. IOP keeps "tariff_ids" attached to the first connector of the EVSE in the payload and applies them to the EVSE. "tariff_ids" of other connectors are ignored.
-   Gireve extends "Tariff" object with 2 new properties target_operator_country_code and target_operator_party_id enabling a CPO to differentiate tariff description per eMSP. [See Differentiate tariff per eMSP.](#differentiate-tariff-per-emsp)
-   A tariff shall be immutable, that’s why Gireve suggests CPOs  never update a tariff description, except for text properties and "end_date".
