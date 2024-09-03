### [<- Back to module selection](cpo_edits.md)

# `Connection And Register specifications`

IOP follows the OCPI 2.2.1 standard for Connection & Register process. [See OCPI 2.2.1 specifications.](https://github.com/ocpi/ocpi/blob/release-2.2.1-bugfixes/credentials.asciidoc)
The Credentials module is used to manage the OCPI connection between 2 OCPI platforms. It doesn’t require the usage of Headers-From and Headers-To because it is not used for an operator to communicate.
Unlike OCPI 2.1.1, the Authorization token shall be base 64 encoded in OCPI 2.2.1.

**Also, Gireve presents the role “HUB” and will only presents the Operator FR*007 (Production environment) and or FR*107 (Preproduction environment).**

#### Information and requirements

-   Gireve has not implemented the “update Credentials”. To update the OCPI connection information (i.e. Authorization Token and/or endpoints) the platform shall unregister with IOP then register again.
-   During the registration process, a Partner can present a list of operators, but it will be ignored by IOP. Gireve manually creates/integrates the various operators (who have contracted with Gireve) a Partner can manage.
-   Only 1 OCPI handshake is needed to initialise the connection and enable the communication between several operators hosted by a unique platform and IOP.
-   The usage of Headers-To and Headers-From is mandatory for Gireve, except for Credentials module.
-   Gireve has only implemented the “Register – FromIOP” and “Unregister – FromIOP” use cases.
