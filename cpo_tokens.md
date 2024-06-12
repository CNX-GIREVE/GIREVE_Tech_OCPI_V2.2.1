### [<- Back to module selection](cpo_edits.md)

# Contents

* [Tokens module specifications](#tokens-module-specifications)
  - Download of Tokens not recommended
  - "LocationReferences" mandatory in POST Tokens Authorize requests

***

# `Locations module specifications`

IOP follows the OCPI standard for Tokens module. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_tokens.asciidoc)

## Download of Tokens not recommended
Unlike OCPI 2.1.1, CPOs connected to Gireve in OCPI 2.2.1 don’t need to download the whole list of eMSP Tokens.
In case they don’t know the Token, the CPO requests IOP with a “POST Tokens-authorize” then receive the full description of the Token if the eMSP is connected to Gireve.
For this reason, Gireve doesn’t include the download of Tokens by CPOs in its current implementation of OCPI 2.2.1.

## "LocationReferences" mandatory in POST Tokens Authorize requests
As in OCPI 2.1.1, when requesting authorization, the CPO :
-   Must specify 1 Location.
-   Can set 0 to N EVSEs in its request. Gireve will select 1 and only 1 to continue the authorization request.

### Information and requirements
-   Gireve suggests that CPOs do not download Tokens of eMSPs and to send a POST Tokens authorize request in case of unknown Tokens. For this reason, Gireve doesn’t include the download of Tokens by CPOs in its current implementation of OCPI 2.2.1.
-   The POST Tokens authorize request shall contain the reference to a Location.
