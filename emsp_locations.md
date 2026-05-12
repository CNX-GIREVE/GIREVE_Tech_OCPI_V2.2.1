### [<- Back to module selection](emsp_edits.md)

 
# Contents 
* [Locations module specifications](#locations-module-specifications)
  - Static and dynamic attributes
  - PULL Locations: Retrieve Locations of a single given CPO
  - PULL Locations ToIOP: Get List, Full and Delta modes
  - PULL Locations ToIOP: evse_id
  - PULL Locations ToIOP: “gireve_id” An Extra Gireve Property
  - PULL Locations ToIOP: tariff_ids
  - PULL Locations ToIOP: Plug&Charge (P&C) : Connector Object - new attribute “capabilities”
  - PULL Locations ToIOP: Connector Standards
  - Fields Not Implemented by Gireve
  - PUSH Locations FromIOP

***

# `Locations module specifications`

IOP follows the OCPI 2.2.1 standard for Locations upload by a CPO. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_locations.asciidoc)

## Static and dynamic attributes

The attributes of the Location object are of 2 types :

-   Static attributes are data attributes that do not change frequently (address, localisation …). These data are integrated in Gireve database through the Gireve quality process and could take some time before to be stored by Gireve and displayed to eMSPs.
-   Dynamic attributes are data attributes that may change frequently (availability, occupied/free …). In OCPI 2.2.1, only **<ins>“EVSE.status”</ins>** and **<ins>“Connector.tariff_ids”</ins>** are considered as **dynamic**. These data are integrated in real-time by Gireve when CPOs send updates to IOP.

## PULL Locations: Retrieve Locations of a single given CPO 

When an eMSP requests IOP to get Locations, IOP responds with Locations of every CPOs the eMSP is in contract with.
In some cases, eMSPs need only Locations of a specific CPO. For example, when the eMSP initializes data of a CPO after signature of a new roaming agreement.
This use case is open to eMSPs by filling the 2 headers **“ocpi-to-country-code”** and **“ocpi-to-party-id”** by CPO’s country_code and party_id.

## PULL Locations ToIOP: Get List, Full and Delta modes

If the eMSP wants to retrieve all the Locations, it should not include **« date_from »** and/or **« date_to »** parameters in its request. But if it wants to get only changes (optimised), it should use it.

**<ins>NB</ins>**: The Location and EVSE deletion logic is different if using « date_from » and/or « date_to» parameters or not. When « date_from » and/or « date_to» are present, the eMSP get EVSEs of the Location in status « REMOVED » whereas without « date_from » and/or « date_to» parameters the deleted items are not included in the response (see table below).

For information, when eMSP PULL Locations list from IOP, IOP follows the below logic in responses provided :

| LOCATION |	EVSE | CONNECTOR |
| ----------- | ----------- | ----------- |
| CREATE | New Location is part of the response. | New EVSE is part of the response. | New Connector is part of the response. |
| UPDATE	| Updates are parts of the response. | Updated EVSE is part of the response. |NUpdated Connector is part of the response. |
| DELETE |	- Using « date_from » and/or « date_to» option, response contains all EVSEs of the Location with « REMOVED » value for « status » field. - Without « date_from » and/or « date_to» option, the response does not contain this Location. - Using « date_from » and/or « date_to» option, response contains the EVSE with « REMOVED » value for « status » field. - Without « date_from » and/or « date_to» option, the response does not contain this EVSE. |	Deleted Connectors are not part of the response. |

If the eMSP wants to retrieve a list of Locations, it can call the URL: /ocpi/sender/2.2.1/locations?date_from= using the paginated properties date_from, date_to, offset and limit.

Parameters « offset » and « limit » are optional but IOP always returns a paginated response (subset of objects list and link, X-Total-Count and X-limit headers).
The eMSP must call **the link returned in the headers** to get the next pages. 

IOP has its own max limit **(100 Locations)** and answers with its if the client limit is upper than IOP one or the client doesn’t set its limit. 

## PULL Locations ToIOP: evse_id

It may happen that evse_id may not be compliant with the eMI3 standard. 

> :warning: <ins>**In this case, you should not reject them because of the non-compliance with the standard.**</ins>

## PULL Locations ToIOP: “gireve_id” An Extra Gireve Property

In OCPI **2.1.1**, Gireve uses its internal ID (Gireve ID) to identify a location, EVSE, or connector when transmitting location data to an eMSP.

In OCPI **2.2.1**, the availability of the country_code and party_id fields allows the eMSP to identify the CPO owning the location. As a result, Gireve will send external IDs, as provided by the CPO in the OCPI fields, instead of internal IDs to eMSPs connected via OCPI 2.2.1.

In addition to the external ID, Gireve will also include a new field (not part of the OCPI protocol), called gireve_id, for each of the following elements : **Location / EVSE / Connector.**
It is useful for an eMSP upgrading from OCPI 2.1.1 to OCPI 2.2.1 to reconcile the location data received via these two protocols.

## PULL Locations ToIOP: tariff_ids

When the eMSP download the locations, it receives only tariffs_id with the type “REGULAR” (B2B) and they are mapped with external_id of tariff(s) as sent by CPOs.

## PULL Locations ToIOP: Plug&Charge (P&C): Connector Object - new attribute “capabilities”

The ability to enable Plug & Charge (P&C) functionality, eliminating the need for a physical badge, offers a significant improvement in the user experience for EV drivers.

In alignment with the OCPI 2.3 white paper, which recommends adding Plug & Charge capabilities at the Connector level rather than the EVSE level (as currently defined in OCPI 2.1.1 and 2.2.1), Gireve has implemented a mechanism enabling CPOs to inform eMSPs about EVSE compatibility with Plug & Charge.

To achieve this, Gireve has introduced a new attribute, capabilities, at the Connector object level. This attribute accepts the following list of values:

| OCPI Field |	Enum | Description |
| ----------- | ----------- | ----------- |
| Location.evse.connector.capabilities | ISO_15118_2_PLUG_AND_CHARGE | Compatibility of an EVSE with PnC using ISO15118-2 | 
| Location.evse.connector.capabilities	| ISO_15118_20_PLUG_AND_CHARGE | Compatibility of an EVSE with PnC using ISO15118-20 |

> :warning: <ins>**eMSPs should accept these two new capabilities when they are provided by Gireve.**</ins>

## PULL Locations ToIOP: Connector Standards

Gireve supports all connector standards, including US types, and ensures they are communicated to eMSPs.

## Fields Not Implemented by Gireve

**<ins>Publish_allowed_to</ins>**
The “publish_allowed_to” field is not implemented by Gireve; therefore, eMSPs will not receive it.
Instead, eMSPs will only receive the publish field for a location. 
eMSPs may comply OCPI requirement for a publish false Location and not publish it.

**<ins>Opening_times.exeptional_openings</ins>**
The “exceptional_openings” field of the “Opening_times” object is not implemented by Gireve; therefore, eMSPs will not receive it.

**<ins>Charging_when_closed</ins>**
The “charging_when_closed” field is not implemented by Gireve; therefore, eMSPs will not receive it.

**<ins>Images</ins>**
The “images” field is not implemented by Gireve; therefore, eMSPs will not receive it.

**<ins>Energy_mix</ins>**
The “energy_mix” field is not implemented by Gireve; therefore, eMSPs will not receive it.


## PUSH Locations FromIOP

When the status of an EVSE changes, the CPO will send the update to Gireve dynamically.

Gireve then forwards the dynamic status changes of the EVSE using the external IDs of the location and EVSE. The uniqueness of these IDs is guaranteed by the combination of the country_code and party_id of the CPO in the request URL.


### Informations and Requirements

-  By filling the 2 headers “ocpi-to-country-code” and “ocpi-to-party-id” by the CPO’s country_code and party_id, the eMSP is able to retrieve only the locations of this specific CPO.

-  The Location and EVSE deletion logic is different if using « date_from » parameter or not. When « date_from » is present, the eMSP get EVSEs of the Location in status « REMOVED » whereas without « date_from » the deleted items are not included in the response. 

 -  In addition to the CPO system ID, Gireve will also include a new field called gireve_id for each location, EVSE, and connector.

-  When the eMSP downloads the locations, it receives only the tariff_id with the type " REGULAR" (B2B), and these are mapped with the tariff ID(s) as sent by the CPOs.

-  Gireve has implemented a mechanism allowing CPOs to inform eMSPs about EVSE compatibility with Plug & Charge by introducing two new capabilities at the EVSE level: Location.evse.connector.capabilities. ISO_15118_2_PLUG_AND_CHARGE and Location.evse.connector.capabilities. ISO_15118_20_PLUG_AND_CHARGE. eMSPs should accept these two new capabilities when they are provided by Gireve.

-  All connector standards, including US types, are supported by Gireve and communicated to eMSPs.

-  The fields “Publish_allowed_to”, “Opening_times.exeptional_openings”, “Charging_when_closed”, “Images” and “Energy_mix” are not implemented by Gireve, therefore, the eMSP will not receive it.

-  Gireve forwards the dynamic status changes of the EVSE using the CPO IDs of the location and EVSE.
