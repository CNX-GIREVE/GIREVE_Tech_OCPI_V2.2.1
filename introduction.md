# Contents
## [Introduction](#introduction)
* [Aims](#aims)
* [Intended Audience](#intended-audience)
* [Definitions and Abbreviations](#definitions-and-abbreviations)
* [Cardinality expression](#cardinality-expression)

***

# Introduction

## `Aims`

This document describes guidelines to perform a proper connection with the Gireve’s platform using the OCPI protocol version 2.2.1.

## `Intended Audience`

This document is dedicated to technical teams (system administrators, developers, etc.) of systems connected or to be connected to the Gireve’s platform through OCPI.

This document also covers some operational requirements that must be considered when implementing the OCPI 2.2.1 protocol.

## `Definitions and Abbreviations`

| Word | Meaning |
| ----------- | ----------- |
| **IOP** | Inter-operation Platform. **IOP** is the acronym of the Gireve’s eMobility Services Platform. |
| **RPC** | Référentiel des Points de Charge (in French) = Charge Points Repository (in English) The **RPC** is a system, built around a database that contains Electric Vehicles Charge Infrastructure (EVCI) description. It is connected to **IOP**. **IOP**’s interfaces (eMIP, OCPI …) are the only way to access **RPC**, for partners systems. |
| **ToIOP** | Referring to flows for which an operator requests Gireve platform **IOP**. Partner system is client. IOP is server |
| **FromIOP** | Referring to flows for which Gireve platform **IOP** requests an operator. **IOP** is client. Partner system is server |
| Platform | A platform is a backend communicating with IOP through OCPI. A platform can manage one to several operators. |
| Operator | An operator is a business entity as a CPO or an eMSP and supervised by a platform. |
| Headers-To | Refers to OCPI 2.2.1 headers “OCPI-to-country-code” and “OCPI-to-party-id” |
| Headers-From | Refers to OCPI 2.2.1 headers “OCPI-from-country-code” and “OCPI-from-party-id” |

## `Cardinality expression`

To designate the cardinality of fields on data structures the following symbols are used:

| Symbol | Description |
| ----------- | ----------- |
| ? | Optional |
| 1 | Mandatory |
| * | 0 to n occurrences |
| + | 1 to n occurrences |

Gireve’s data integration process includes quality controls that ensure the consistency of its data referential. The standard data quality level of Gireve is sustained by numerous mandatory attributes that are not all mandatory by the OCPI standard.

#### Information and Requirements
-   Note that some attributes may not be mandatory by the OCPI standard but are required by Gireve for quality reasons. For further information about the cardinality of attributes by Gireve’s quality standard, please refer to the document “Starting roaming operations as a CPO”.  



