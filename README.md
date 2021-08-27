<p align="center">
  <img src="https://user-images.githubusercontent.com/68013812/94918899-c672c700-04b3-11eb-9132-7125bbf77fa5.png"
   alt="drawing" style="width:300px;"/>
</p>
## HelloID-Conn-Prov-Target-NedapONS-Employee

<!-- TABLE OF CONTENTS -->
## Table of Contents
* [Introduction](#introduction)
* [Getting Started](#getting-started)
  * [Connection Settings](#Connection-Settings)
  * [Prerequisites](#Prerequisites)
  * [Remarks](#Remarks)
* [Provisioning](#provisioning)
* [Fact Sheet](#Fact-Sheet)
  * [Remote Nedap documentatie](#Remote-Nedap-documentatie)
* [Setup the connector](Setup-The-Connector)
* [HelloID Docs](#helloid-docs)
* [Forum Thread](#forum-thread)



## Introduction
This Repository does only contains the readme. The source code can be found in a private Repoistry and is meant only for internal use. Link to Repoistry: [Nedap Ons Employee](https://github.com/Tools4everBV/HelloID-Conn-Prov-Target-NedapONS-Employee)

Nedap Ons provides an API and XML import method to programmatically interact with its services and data. This connector can manage the employees in Nedap. It does not support scheduling or other non Identity and Access management functionality. It can create multiple employees accounts in Nedap for each employment (EmployeeId and Employment sequence number combination).


<!-- GETTING STARTED -->
## Getting Started

### Connection Settings

The following settings are required to connect to the API.

| Setting     | Description |
| ------------ | ----------- |
| Environment URL API     |    https://api-staging.ons.io                                     |
| Certificate (.PFX) Path    |  <Fullpath to Certificate> Nedap-cert.pfx                       |
| Certificate Password |    Password of the certificate                                       |
| IO Import Url | The Url of the XML import of Nedap, Example: https://<.ioservice.net/importws/import"   |
| IO Import Username |   The username of the IO Import         |
| IO Import Password|   The Password of the IO Import          |
| mappingFilePath| The Path to the mapping file   |
| CSV separation Character| Mapping File CSV Separation Character         |

### Prerequisites
- A valid Nedap certificate (.PFX) *Tools4ever need to requests a certificate by Nedap to access the REST API*
- Credentials for the IO Import *Different account credentials as the REST API*
- Mapping between HR departments/functions to Cluster/ Education/ registrationProfile
- An custom property on the HelloID Person contract with a combination of the employeeCode and EmploymentCode named: [custom.NedapOnsIdentificationNo]
Example:
  ```javascript
  function getValue() {
      return sourceContract.PersonCode + "-" + sourceContract.EmploymentCode
  }
  getValue();
  ```


### Remarks
- This connector does only manage the Employee object, You can use this connector in combination with the Nedap-Users connector. When you use them both in the same environment. You must add the Employee Target system as "Use account data from system". To make sure the employee object exists in Nedap before starting to create the account object.
- The connector is built. Containing three properties which cannot be mapped directly from the person model in HelloID. So there is a mapping file needed. This file must include a mapping between HR departments and/or function to a Nedap cluster, education and registration profile. The actions of the connector fails, when there is no mapping found. Of course, this can be changed in the code. But is not configurable by default.
- When updating an employee account, you cannot verify if a property is successfully updated in Nedap. To do this you must check the Import Rapportage from the Nedap UI. *Beheer > Import > Importrapportage inzien*


## Provisioning
Using this connector you will have the ability to create and manage the following items in Nedap:

Create:
* Multiple employee objects for each Employement (employeeId + SequenceNumber), based on the contracts in condition from the Business Rules

Update:
* Update attributes of the corresponding employee account as configured in the connector mapping.
* Create new employee account for each Employement (employeeId + SequenceNumber) combination.
* Disable account reference from Aref  -*See delete action* -

Delete:
*  Set an end date of the contract for each employee account. So the user is unable to log in.


Nedap Employee Mapping:
| Header    | Description |
| ------------ | ----------- |
| Title.ExternalId   | Property of the HelloID primary contract
| Department.ExternalId  | Property of the HelloID primary contract
|EducationId   |   Deskundigheidsprofiel > Import Code
| RegistrationProfile | Weekkaartprofiel > ProfileName
| ClusterId  | Organigram > Identificatie
| ClusterName  |    Organigram > Naam

*Please note! That the mapped value will be created if they do not exist in Nedap! So if you choose in a RegistrationProfile that does not exist, it will be created. What might encounter some unexpected behavior.*

## Fact Sheet
The following table displays an overview of the functionality for the Nedap Ons connector for HelloID Provisioning and Service Automation.

|Type of action| Nedap    | HelloID provisioning |HelloID Service Automation|
| ------------ | ----------- |----------- |----------- |
| Create Employees |Yes|Yes, *Simple employee, no scheduling* |No
|Update Employees  |Yes|Yes, *Simple employee, no scheduling* |No
|Delete Employee |No|No, *Sets an enddate on contract* |No
|Manage Employee Contracts|Yes|Yes, *Simple employee, no scheduling* |No
| Set RegestrationProfiel (Weekkaart) |Yes|Yes, *Additional mapping required*|No
|Set Cluster (Team)|Yes|Yes, *Additional mapping required*|No
|Set education (Deskundigheidsprofiel )|Yes|Yes, *Additional mapping required*|No
|Set Discipline |Yes|No|No
|Set Profession |Yes|No|No
|VacationRight|Yes|No, *outside the scope of identity management.*|No
|AccountAmount|Yes|No, *outside the scope of identity management.*|No
|VacationAmounts|Yes|No, *outside the scope of identity management.*|No
|hourlyWages|Yes|No, *outside the scope of identity management.*|No
|CollectiveAgreement|Yes|No, *outside the scope of identity management.*|No
|FreeField|Yes|No|No
|Set Dashboard profiel |No|No|No


### Remote Nedap documentatie
* Nedap API documentatie → [klik](https://www.ons-api.nl/APIS.html)
 * Nedap XML import handleiding → [klik](https://support.nedap-healthcare.com/topic/gegevens-importeren-in-ons-via-xml-bestanden#te-importeren-gegevens-en-schema-definitie-(xsd))
* Nedap XML import documentatie → [klik](https://support.nedap-healthcare.com/images/technische_documenten/io_server_xml_interface.pdf)
* Nedap ONS autorisatie handleiding → [klik](https://ons-api.nl/support/Shield.html)

## Setup the connector

Before using this connector, make sure you replace the variables below. So they meet your configuration:
 <img src="assets/configuration.png">



_For more information about our HelloID PowerShell connectors, please refer to our general [Documentation](https://docs.helloid.com/hc/en-us/articles/360012558020-How-to-configure-a-custom-PowerShell-target-connector) page_


## HelloID Docs
The official HelloID documentation can be found at: https://docs.helloid.com/

## Forum Thread
The Forum thread for any questions or remarks regarding this connector can be found at: [Helloid-prov-target-nedap-ons-employee](https://forum.helloid.com/forum/helloid-connectors/provisioning/312-helloid-prov-target-nedap-ons-employee)