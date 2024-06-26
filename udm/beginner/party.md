Recommeded reading
https://www.moqui.org/m/docs/mantle/Mantle+Structure+and+UDM/Party

Introduction to Party Data Model

This document provides an overview of this model, focusing on its core entities - Party, Person, and PartyGroup - and their applications in representing various business relationships. Additionally, we will delve into Party Roles, contact mechanisms, and their purposes. Finally, we provide sample JSON data for a Person as a Customer and a PartyGroup as a Supplier.
Party Data Model Overview

Entities
1. Party
* Description: Represents an individual, a group, or an organization that participates in business processes. Parties can play different roles in various contexts. Parties have various comunication channels associated with them such as email addresses, phone numbers, postal addresses.
* Key Attributes: Unique identifier (partyId), partyType.
2. Person
* Description: A specialization of Party, representing individual human beings.
* Key Attributes: Inherits partyId, having attributes like firstName, lastName, birthDate.
* Relationship with Party: Every Person is a Party, but with additional attributes specific to individuals.
3. PartyGroup
* Description: Another specialization of Party, representing collective entities such as companies or organizations.
* Key Attributes: Inherits partyId, having attributes like groupName.
* Relationship with Party: Every PartyGroup is a Party, but with attributes specific to groups.
4. Party Roles
* Function: Defines the role of a Party in a specific context. Each party may have zero to many roles that defines how party is related to other structures or even other parties.
* Examples: Customer, Supplier, Employee.
* Assignment: A Party is assigned a Role through the PartyRole entity, linking the Party to its function within the business ecosystem.
5. Contact Mechanisms and Purposes

Adding Contact Mechanisms
* Types: Includes phone numbers (TelecomNumber) and postal addresses (PostalAddress).
* Creating Contact mechanism: Achieved via ContactMech entity, which stores the contactMechId and contactMechTypeId, further phone numbers are stored in the TelecomNumber uniquely identified by the contactMechId and for postal addresses are stored in the PostalAddress entity uniquely identified by the contactMechId. TelecomNumber and PostalAddress have one-to-one relationship with ContactMech.
* Linking to Party: Achieved via the PartyContactMech entity, which associates a contact mechanism with a Party through a unique contactMechId.

Assigning Purposes to Contact Mechanisms
* Function: Defines the specific use of a contact mechanism, like billing or shipping.
* Implementation: Utilizes the PartyContactMechPurpose entity which is a association entity of PartyContactMech and ContactMechPurposeType, linking a contactMechId to a purpose such as BILLING or SHIPPING in context of partyId.

Sample JSON Data
1. Person as Customer
```json
{
  "Party": {
    "partyId": "CUST123",
    "partyTypeId": "PERSON"
  },
  "Person": {
    "partyId": "CUST123",
    "firstName": "John",
    "lastName": "Doe",
    "birthDate": "1990-01-01"
  },
  "PartyRole": {
    "partyId": "CUST123",
    "roleTypeId": "CUSTOMER"
  },
  "ContactMech": [
    {
      "contactMechId": "PHONE001",
      "contactMechTypeId": "TELECOM_NUMBER"
    },
    {
      "contactMechId": "ADDR001",
      "contactMechTypeId": "POSTAL_ADDRESS"
    }
  ],
  "TelecomNumber": {
    "contactMechId": "PHONE001",
    "countryCode": "+1",
    "contactNumber": "555-1234"
  },
  "PostalAddress": {
    "contactMechId": "ADDR001",
    "address1": "123 Elm Street",
    "city": "Springfield",
    "postalCode": "12345",
    "countryGeoId": "USA"
  },
  "PartyContactMech": [
    {
      "partyId": "CUST123",
      "contactMechId": "PHONE001"
    },
    {
      "partyId": "CUST123",
      "contactMechId": "ADDR001"
    }
  ],
  "PartyContactMechPurpose": [
    {
      "partyId": "CUST123",
      "contactMechId": "ADDR001",
      "contactMechPurposeTypeId": "BILLING_LOCATION",
      "fromDate": "2024-01-22 00:00:00"
    },
    {
      "partyId": "CUST123",
      "contactMechId": "ADDR001",
      "contactMechPurposeTypeId": "SHIPPING_LOCATION",
      "fromDate": "2024-01-22 00:00:00"
    }
  ]
}
```


2. PartyGroup as Supplier
```json
{
  "Party": {
    "partyId": "SUPP456",
    "partyTypeId": "PARTY_GROUP"
  },
  "PartyGroup": {
    "partyId": "SUPP456",
    "groupName": "XYZ Supplies Inc."
  },
  "PartyRole": {
    "partyId": "SUPP456",
    "roleTypeId": "SUPPLIER"
  },
  "ContactMech": [
    {
      "contactMechId": "PHONE002",
      "contactMechTypeId": "TELECOM_NUMBER",
      "infoString": "555-6789"
    },
    {
      "contactMechId": "ADDR002",
      "contactMechTypeId": "POSTAL_ADDRESS"
    }
  ],
  "TelecomNumber": {
    "contactMechId": "PHONE002",
    "countryCode": "+1",
    "contactNumber": "555-6789"
  },
  "PostalAddress": {
    "contactMechId": "ADDR002",
    "address1": "456 Oak Avenue",
    "city": "Metropolis",
    "postalCode": "54321",
    "countryGeoId": "USA"
  },
  "PartyContactMech": [
    {
      "partyId": "SUPP456",
      "contactMechId": "PHONE002"
    },
    {
      "partyId": "CUST123",
      "contactMechId": "ADDR002"
    }
  ],
  "PartyContactMechPurpose": [
    {
      "partyId": "SUPP456",
      "contactMechId": "ADDR002",
      "contactMechPurposeTypeId": "BILLING_LOCATION",
      "fromDate": "2024-01-22 00:00:00"
    },
    {
      "partyId": "SUPP456",
      "contactMechId": "ADDR002",
      "contactMechPurposeTypeId": "SHIPPING_LOCATION",
      "fromDate": "2024-01-22 00:00:00"
    }
  ]
}
```



