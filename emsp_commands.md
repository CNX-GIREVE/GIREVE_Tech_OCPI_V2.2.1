### [<- Back to module selection](emsp_edits.md)

# Contents

* [Commands Module specifications](#commands-module-specifications)
  - “ocpi-to-country-code” and “ocpi-to-party-id” headers mandatory in StartSession and StopSession commands
  - “evse_uid” mandatory in StartSession command
  - “connector_id” optional in StartSession command
  - ReserveNow command
  - CancelReservation command
  - UnlockConnector command

***

# `Commands Module Specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_commands.asciidoc)

## “ocpi-to-country-code” and “ocpi-to-party-id” headers mandatory in StartSession and StopSession commands

eMSPs now receive locations with CPO IDs. Therefore, when an eMSP sends to IOP a start or stop session command, it is necessary to identify the CPO owner of the location by filling the 2 headers **“ocpi-to-country-code”** and **“ocpi-to-party-id”** by the CPO’s country_code and party_id.

## “evse_uid” mandatory in StartSession command

In OCPI 2.2.1 standard, the “evse_uid” property is optional for StartSession command.
IOP requires it to do the mapping with the eMIP protocol.

## “connector_id” optional in StartSession command

The connector_id field is optional in OCPI 2.2.1. If provided, it will be transmitted to the CPO in the start command.

## ReserveNow command

The ReserveNow command is not implemented by IOP.

## CancelReservation command
The ReserveNow command is not implemented by IOP.

## UnlockConnector command
The UnlockConnector is not implemented by IOP. 

### Information and Requirements

-  The "evse_uid" property is optional for the StartSession command, in OCPI 2.2.1standard. However, IOP requires it for mapping with the eMIP protocol.

-  In OCPI 2.2.1, the “connector_id” property is optional. If specified, it will be forwarded to the CPO in the start command.
