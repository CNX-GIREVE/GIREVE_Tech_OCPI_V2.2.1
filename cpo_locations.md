### [<- Back to module selection](cpo_edits.md)


# Contents 
* [Locations module specifications](#locations-module-specifications)
  - Locations information required by Gireve
  - Static and dynamic attributes
  - "tariff_ids" property
  - "publish" property
  - Store and Forward – PUT and PATCH Locations

***

# `Locations module specifications`

IOP follows the OCPI 2.2.1 standard for Locations upload by a CPO. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_locations.asciidoc)

## Locations information required by Gireve

Gireve requires some information to be filled by CPOs in Locations data, even if they are optional in the OCPI 2.2.1 standard. This information is used to ensure data quality.

## List of the required properties

| Property |	Reason |
| ----------- | ----------- |
| owner.name | Used to fill the information about the owner of the land where the charging station is located. The owner can also manage the charging station. |
| operator.name	| Used to fill the “brand name” information. |
| evses.connectors.tariff_ids |Used to dispatch EVSEs per tariff groups, even in case Tariffs are described through the Gireve’s connect place. |
| evses.capabilities |	Used to know, among others, if the EVSE is RFID or remote start capable. |

## Static and dynamic attributes

The attributes of the Location object are of 2 types:
-   Static attributes are data attributes that do not change frequently (address, localisation …). These data are integrated in Gireve database through the Gireve quality process and could take some time before to be stored by Gireve and displayed to eMSPs.
-   Dynamic attributes are data attributes that may change frequently (availability, occupied/free …). In OCPI 2.2.1, only **<ins>“EVSE.status”</ins>** and **<ins>“Connector.tariff_ids”</ins>** are considered as **dynamic**. These data are integrated in real-time by Gireve when CPOs send updates to IOP.

## "tariff_ids" property

Gireve uses the “tariff_ids” information provided by CPOs in Locations to dispatch CPO’s EVSEs into separated EVSE tariff groups. Also, CPOs can refer to these tariff groups when they describe tariffs directly in their roaming offer via Gireve’s connect place.
If CPOs use the OCPI Tariffs module to send their tariffs, the management of tariffs and relations to the charging infrastructure follows the OCPI standard except that in Gireve systems, tariffs are linked to EVSEs and not to connectors.
In its current implementation of OCPI 2.2.1, Gireve stores all tariffs coming from CPOs, whatever their nature, but transfers only **<ins>“B2B Regular”</ins>** tariffs to eMSPs, whatever their protocol.

## "publish" property
The “publish” information, added in OCPI 2.2.1 on Locations level, is used by CPO to inform other parties that the Location shall not be displayed on any support (i.e. a map on mobile application, …).
As this information doesn’t exist in OCPI 2.1.1, Gireve doesn’t send Locations with **<ins>“publish”</ins>** value false to eMSPs connected to IOP in OCPI 2.1.1.

## Store and Forward – PUT and PATCH Locations
A Store and Forward mechanism shall be implemented by CPOs to ensure that no data upload may be lost, in case of a connection loss. Any data upload that didn’t get a correct response (HTTP code: 2xx) from  Gireve IOP platform  must be stored on CPO side and a retry process must be active. After the connection recovery, the Data Upload messages must be resent in a FIFO manner.

### Information and requirements
-   Locations static data follow a quality process before they are integrated in Gireve’s repository, meaning that Locations creation, update or deletion could take some time before being integrated by Gireve then displayed to other parties.
-   As a reminder, the “evse_id” property is mandatory except if the EVSE has status “REMOVED”. Moreover, this property containing the eMI3Id of the EVSE shall begin by the eMI3Id of the CPO.
-   "connectors.tariff_ids", “owner.name”, “operator.name” and “evses.capabilities” are mandatory for Gireve.
-   Locations having “publish” flag set to false are not transferred to parties connected to Gireve in OCPI 2.1.1 protocol.
-   In Gireve’s model, the tariff is attached to the EVSE level and not to the connector level as in OCPI. IOP keeps “tariff_ids” attached to the first connector of the EVSE in the payload and applies them to the EVSE. “tariff_ids” of other connectors are ignored.
-   A CPO shall retry every PUT and PATCH Locations in case of a technical error (i.e. timeout, http return code different than 2xx, …) 
