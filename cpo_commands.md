### [<- Back to module selection](cpo_edits.md)

# Contents

* [Commands Module specifications](#commands-module-specifications)
  - List of available Commands
  - New field "connector_id" in START_SESSION

***

# `Commands Module Specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_commands.asciidoc)

## List of available Commands

| Commands | Available ? |
| ----------- | ----------- |
| START_SESSION	 | **Yes** |
| STOP_SESSION	| **Yes** |
| RESERVE_NOW	| No |
| CANCEL_RESERVATION |	No |
| UNLOCK_CONNECTOR	| No |

## New field “connector_id”  in START_SESSION

A new field **“connector_id”** was added to the START_SESSION request in OCPI 2.2.1. As it is not present in OCPI 2.1.1 , eMSPs connected to IOP in OCPI 2.1.1 are not able to send this information, even if it is mandatory for CPO.
