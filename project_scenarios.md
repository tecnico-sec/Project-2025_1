Instituto Superior Técnico, Universidade de Lisboa

**Network and Computer Security**

# Preamble

This document explains the project scenarios.  
Please read the project overview first.

The scenarios describe a richer application context than the one that will be actually needed to be implemented.  
The security aspects should be the focus of your work.
User interface design, although critical in a real application, is secondary in this project.

Each project team can change the document format for each scenario, but the fields shown must still be included in some way.

# Project Scenarios

_Congratulations! You have been hired by one of the following companies._

Each company has its business context and, more importantly, a document format that is pivotal for their business.  
Later in the project, a security challenge will be revealed and your team will have to respond to it with a suitable solution.

----

# Scenarios

- [NotIST](#NotIST)
- [TicketIST](#TicketIST)
- [MotorIST](#MotorIST)
- [MessagIST](#MessagIST)

----

## NotIST
NotIST is a note application that allows users to create personal notes and shareable notes. It’s a local first application, meaning that notes are first stored in the local device, and are periodically synced to an external server for backups. A new functionality of NotIST is its ability to share notes with other users. NotIST is built to ensure privacy. Notes, even the ones stored locally, are always encrypted in such a way that only its owner can decrypt them. Shared notes are also encrypted, but any contributor of the notes can decrypt them. 

The data structure of any note is a JSON document with the following structure:

```json
{
  "id": 123,
  "title": "Example Document",
  "note": "This is an example document.",
  "data_created": "2022-01-01T12:00:00Z",
  "date_modified": "2022-01-02T12:00:00Z",
  "last_modified_by": 456,
  "version": 3,
  "owner": {
    "id": 456,
    "username": "john"
  },
  "editors": [
    {
      "id": 789,
      "username": "jane"
    },
    {
      "id": 1011,
      "username": "bob"
    }
  ],
  "viewers": [
    {
      "id": 1213,
      "username": "alice"
    },
    {
      "id": 1415,
      "username": "charlie"
    }
  ]
}
```


### Protection Needs

The protected document must ensure the integrity and confidentiality of the data. Only the owner of the notes can see their content and the owner of the notes can verify that they were not tampered with. Assume the notes are stored in the server. 
Ensure the following security requirements are met:
- [SR1: Confidentiality] Only the owner of the notes can see their content.
- [SR2: Integrity 1] The owner of the notes can verify they were not tampered with.
- [SR3: Integrity 2] The owner of the notes can verify if some note is missing.
- [SR4: Authentication] Only the owner of the notes can access them.


### Security challenges
#### Security challenge A
The users of NotIST want to share their notes with anyone on the web. Implement the required functionality to allow sharing a note with other users. 
Ensure the following security requirements are met:
- [SRA1: Authentication] Only authenticated and authorized users can see the content of the notes.
- [SRA2: Integrity 1] Anyone that has access to the note can verify its integrity.
- [SRA3: Integrity 2] It is possible to verify the integrity of the notes throughout their versions.  
#### Security challenge B
Some NotIST users asked a new feature to allow them to exchange highly sensitive notes. These notes can only be opened if a user owns a set of two keys. Moreover, it is assumed that some keys may be lost, as such, if any notes were written using a revoked key then such noes should not be accepted.  
Ensure the following security requirements are met:
- [SRB1: Confidentiality 1] Each note can only be read using a set of at least two keys: a personal key, and a note key. 
- [SRB2: Confidentiality 2] Implement a multi-level access (0 - public, 1 - private, 2 - confidential, 3 - top-secret). Users with higher clearance can read any document up to that level (for example, a user with confidential can read any note that is public, private and confidential but cannot read a top-secret note).
- [SRB3: Integrity] If a key is revoked, then any note signed after its revocation should not be accepted. Previous versions of the note signed when the key was valid are accepted. 

----

## TicketIST
IST is selling tickets for scientific events throughout the year. These tickets are in high demand and being bought by many science enthusiasts around the globe. To accommodate with the demand of the ticket, IST is now selling electronic tickets online that can be validated using a smartphone. The data structure of a ticket is as follows:

```json

{
  "id": 12345,
  "event_id": 67890,
  "seller": {
    "id": 11111,
    "username": "seller_name"
  },
  "buyer": {
    "id": 22222,
    "username": "buyer_name"
  },
  "purchase_date": "2022-01-01T12:00:00Z",
  "price": 50.99,
  "seat": "Row A, Seat 1",
  "validation_code": "ABC123"
}

```

### Protection needs 
The tickets hold some sensitive information (buyer, seller, price and validation code). As such, the confidentiality of these fields must be ensured. Moreover, it is necessary to implement a validation mechanism that allows the verifier (the person in the event) to validate that this is a legit ticket.  The validation mechanism must use some cryptographic operation.  A ticket that was used cannot be reused.
Ensure the following security requirements are met:
- [SR1: Confidentiality] Only the buyer, seller and validator can see the content of the ticket.
- [SR2: Integrity 1] It is possible to validate a ticket using a code.
- [SR3: Integrity 2] If the ticket is illegally modified (for example, a normal seat for a VIP seat), such modification must be detected.
- [SR4: Authentication] If an attacker steals a ticket, then they cannot use it to go to the event.


### Security challenges
#### Security challenge A
The demand for these tickets has skyrocketed and there are uncountable cases of people selling the IST tickets in the black market for a higher price (which is illegal). Implement a buyer / seller protocol that allows the ticket holders to resell their tickets ensuring that they do not sell the tickets for a price higher than its real value. Also, ensure that each buyer can only buy a limited amount of 6 tickets (to avoid scrapers).
Ensure the following security requirements are met:
- [SRA1: Authentication 1] Only authenticated users can buy tickets (users must authenticate themselves using some cryptographic mechanism).
- [SRA2: Authentication 2] A user may sell a ticket to other users, however, the buyer cannot overpass the limit of 6 tickets. 
- [SRA3: Validity] After a buyer sells a ticket they cannot attend the event, even if they stored a copy of the ticket code locally .

#### Security challenge B
The sports teams at IST created the IST Box Seat, a special annual ticket that allows sports enthusiasts to attend any sports event throughout the year. These tickets are very expensive and IST allows the ticket owner to concede the ticket for an event if they cannot attend.
Ensure the following security requirements are met:
- [SRB1: Confidentiality] It is possible to share an annual ticket for a single event without sharing the ticket code.
- [SRB2: Validity] If a user shares his annual ticket for an event, he loses the right to attend the event.
- [SRB3: Authentication] Tickets can only be used by their owner (either the owner of the annual ticket or the person he conceded the ticket).

----

## MotorIST
IST is now selling electric cars with modern management systems. The system allows users to configure the car remotely, such as close/open the car; configure the AC; check the battery level. This is done by a user application installer on the users computer/mobile.
Additionally, and to maintain the car up to date, the car also allows for firmware updates from the manufacturer.

The communication with the car is done using a JSON document with the following data structure:

```json
{
 "carID": "1234XYZ",
 "user": "user1",
 "configuration": {
	"ac": [
   	{ "out1": "789"},
   	{ "out2": "1011"}
	],
	"seat": [
   	{ "pos1": "0"},
   	{ "pos3": "6"}
	]
 }
}
```

### Protection needs
All the communication with the car is sensitive and to assure the drivers and owner data protection according to the RGPD (GDPR) must be secure. External entities cannot change or check the user configurations and only the car manufacturer can update the car firmware. The mechanic cannot see the user configurations (unless he has the key) and can only update the car with firmware authorized by the manufacturer.

The following security requirements must be met:
- [SR1: Confidentiality] The car configurations can only be seen by the car owner.
- [SR2: Integrity 1] The car can only accept configurations sent by the car owner.
- [SR3: Integrity 2] The car firmware updates can only be sent by the car manufacturer.
- [SR4: Authentication] The car manufacture cannot deny having sent firmware updates.


### Security challenges

#### Security challenge A
To facilitate the customization and improve the user experience multiple users/drivers exist. They are identified by their unique keys.

Ensure the following security requirements are met:
- [SRA1: data privacy] One user cannot know the configuration of the other user, but may know some current information of the car. For example, user 1 may not know how many km were done by the previous user, but can see the remaining battery level.
- [SRA2: authorization] An unauthorized user cannot change the configuration of the other user.
- [SRA3: authenticity] It must be possible to audit the car and verify which configuration actions were performed by which users.

#### Security challenge B
The car must have a maintenance mode, which is set by the user. In this mode the car is set to the default configuration.

Ensure the following security requirements are met:
- [SRB1: data privacy] The mechanic cannot see the user configurations, even when he has the car key.
- [SRB2: authorization] The mechanic (when authenticated) can change any parameter of the car, for testing purposes. 
- [SRB3: data authenticity] The user can verify that the mechanic performed all the tests to the car. 

----

## MessagIST
IST students use an instant message application to communicate with the members of the IST community. This app allows users to send and receive text messages in a secure form. The structure of a text message is as follows:

```json
{
  "messages": [
    {
      "sender_istid": "ist1123123",
      "receiver_istid": "ist1321564",
      "timestamp": "2022-01-01T12:00:00Z",
      "content": "Hi! do you know the solution for the SIRS exercise?",
    },
    ]
}
```

### Protection needs
To comply with the GDPR, messages from MessagIST must be encrypted and authenticated. 
Ensure the following security requirements are met:
- [SR1: Confidentiality] Only the sender and receiver of a message can see its content.
- [SR2: Integrity 1] The receiver of a message can verify that the sender did wrote the message.
- [SR3: Integrity 2] It must be possible to detect if there is a missing message or if they are out of order.
- [SR4: Authentication] Messages are only send to their authenticated recipients.

### Security challenges
#### Security challenge A
Basic encryption is enough to comply with GDPR but it is not enough to convince the security experts from IST. As such, in this security challenge you are invited to implement a point to point encryption mechanism. 
Ensure the following security requirements are met:
- [SRA1: Confidentiality] Only sender and receiver can see the content of the messages.
- [SRA2: Confidentiality] There must be a protocol that allows two students to exchange a key (in a secure way). You can assume the existence of a side channel for this. 
- [SRA3: Availability] If a user loses their phone, they must be able to recover the message history. 

#### Security challenge B
Group conversations: some students want to use MessagIST to exchange messages in a group. Users that do not belong to the group cannot see any messages.
Ensure the following security requirement are met:
- [SRB1: Confidentiality 1] Only members of the group can see the content of the messages.
- [SRB2: Confidentiality 2] Members that no longer belong to the group, must not be able to see any future message (they can still see previous messages).
- [SRB3: Confidentiality 3] System administrators have full access to the database (including the tables/collections with the messages exchanged by the users), however, they cannot see the content of the messages.


----

[SIRS Faculty](mailto:meic-sirs@disciplinas.tecnico.ulisboa.pt)
