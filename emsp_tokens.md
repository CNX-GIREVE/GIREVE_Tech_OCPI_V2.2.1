### [<- Back to module selection](emsp_edits.md)

# Contents

* [Tokens module specifications](#tokens-module-specifications)
  - Push Tokens ToIOP
  - Pull Tokens FromIOP

***

# `Tokens module specifications`

IOP follows the OCPI standard for Tokens module. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/mod_tokens.asciidoc)

## Push Tokens ToIOP

CPOs connected to Gireve via OCPI 2.1.1 are required to have eMSPs, regardless of their protocol, transfer their full list of Tokens to Gireve.

> :warning: <ins>**PATCH Token to IOP is not yet implemented.**</ins>


## Pull Tokens FromIOP

Pull Tokens FromIOP is not implemented yet by IOP.

Tokens must be sent for roaming to work with OCPI 2.1.1 CPOs.

### Information and Requirements
-  eMSP tokens must be uploaded to the IOP to enable roaming with CPOs connected via OCPI 2.1.1.
