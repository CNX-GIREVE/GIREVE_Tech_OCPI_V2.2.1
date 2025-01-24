# Contents 
* [CPO Use cases required by Gireve](#cpo-use-cases-required-by-gireve)
* [eMSP Use cases required by Gireve](#emsp-use-cases-required-by-gireve)

***
# `CPO Use cases required by Gireve`
Some use cases are required when connecting to Gireve

## Always required

| Use Case | Why ? |
| --- | --- |
| **Register – FromIOP Or Register - ToIOP** | These use cases are needed to initialise connection between a platform and IOP |
| **Push EVCI - ToIOP** | A CPO connected to Gireve must transfer “in real time” EVSE status changes of its EVSEs and tariff_ids of its Connectors |
| **Pull EVCI - FromIOP** | Gireve wants to be able to refresh EVCI data when needed. |

## If the eMSP implements the “Roaming” feature

| Use Case | Why ? |  
| --- | --- |  
| **Real-Time Authorization - ToIOP** | A CPO should be able to request eMSP through IOP when a driver uses his RFID badge or a P&C Contract certificate to charge. |  
| **Remote start - FromIOP** | Remote authorization and start features on CPO infrastructure are required by Gireve. |  
| **Remote stop - FromIOP** | Remote stop features on CPO infrastructure are required by Gireve. |  
| **Push Sessions - ToIOP** | A CPO must be able to send information about charging sessions through Session objects (charge started, ...). |  
| **Push CDRs - ToIOP** | The CPO must send the CDR in real time after the end of the charging session. |  
| **Push Tariffs - ToIOP** | CPOs must inform in real-time, through IOP, eMSPs about tariff changes. |  

### Information and Requirements

• The implementation and certification of the OCPI Tariffs module depends on the tariff’s strategy of the CPO. It is not required if the CPO describes its tariffs through the Gireve connect place.
• CDRs shall be sent as soon as possible after the end of the charge.





# `eMSP Use cases required by Gireve`
Some use cases are required when connecting to Gireve

## Always required

| Use Case | Why ? |
| --- | --- |
| **Register – FromIOP Or Register - ToIOP** | These use cases are needed to initialise connection between a platform and IOP |

## If the eMSP implements the “Roaming” feature

| Use Case | Why ? |
| --- | --- |
| **Push CDRs - FromIOP** | eMSP shall be able to receive CDRs |
| **Push Tokens – ToIOP** | eMSP Tokens shall be uploaded to IOP to enable roaming with CPOs connected through OCPI 2.1.1 |

### Information and Requirements

• eMSP tokens must be uploaded to the IOP to enable roaming with CPOs connected via OCPI 2.1.1.
• eMSPs must always be able to receive CDRs sent by CPOs as soon as possible after the end of the charge.

