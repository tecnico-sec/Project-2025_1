Instituto Superior Técnico, Universidade de Lisboa

**Network and Computer Security**

# Project Overview

This document describes the organization of the project for the Network and Computer Security / Segurança Informática em Redes e Sistemas (SIRS) course.

You will work in a team of 3 members.

The project is contextualized in one of a set of business scenarios: a notes app, a ticket selling app, a vehicle system, and a messaging app.

The project is organized in 3 parts: secure documents, infrastructure, and security challenge.

The first part - _secure documents_ - will require the design and implementation of a custom cryptographic library.
Each business scenario has a core document format that lacks protection.
Each team will have to use adequate cryptography to protect the document.
We recommend that you get teacher feedback before the implementation begins.

The second part - _infrastructure_ - will require the installation and configuration of servers to support the business scenario.
The expected result is a virtual environment with networks and machines, with adequate firewall configurations, running application and database servers chosen by the team, providing operations that access the secure documents in the context of the scenario.
The communication between the machines will need to be protected using secure channel solutions, such as TLS or SSH.

The third part - _security challenge_ - will introduce new requirements for the business scenario that will impact the design security solution.
It will be up to the team to redesign and extend the previous parts.

The expected final result is all the components working together, to demonstrate an overall secure business solution.

This document will now detail the whole project process, stage by stage.

The whole SIRS project process is organized in sequential stages:

| **Stage**                                                             |
| ----------------------------------------------------------------------|
| [1. Design secure document format](#1-design-secure-document-format)                   |
| [2. Implement secure document format](#2-implement-secure-document-format) |
| [3. Build infrastructure](#3-build-infrastructure)             |
| [4. Protect server communications](#4-protect-server-communications)           |
| [5. Respond to security challenge](#5-respond-to-security-challenge)              |
| [6. Prepare demonstration](#6-prepare-demonstration)                 |
| [7. Complete report](#7-complete-report)                             |
| [8. Submit code and report](#8-submit-code-and-report)             |
| [9. Check presentation session](#9-check-presentation-session)     |
| [10. Present and defend](#10-present-and-defend)                     |

<br />

Each stage is described in the following subsections.

# 1. Design secure document format

After having a scenario assigned, your team should design and implement a library to provide cryptographic security to the document format, according to the scenario instructions.

It is highly recommended that the design is presented to the lab teacher before implementation begins.

The library should provide, at least, the following operations:

- `protect()` - add security to a document;
- `check()` - verify security of a document;
- `unprotect()` - remove security from a document.

# 2. Implement secure document format

The choice of programming language and cryptographic library  is up to the team.

Plan a gradual implementation of the solution.
Attend the lab sessions to present your ongoing development and receive early feedback from the teacher.

Generate a set of _dummy_ keys from _dummy_ users to quickly test your security functions.

The library should also provide a command-line interface with, at least, the following commands:

- `(tool-name) help`
- `(tool-name) protect (input-file) (dummy-key) (output-file) ...`
- `(tool-name) check (input-file)`
- `(tool-name) unprotect (input-file) (dummy-key) (output-file) ...`

The `help` command should print an explanation of all the available commands and their arguments.
If the tool is called with any argument missing or with invalid arguments, it should print a helpful message.
In the case of any error, information should be printed, first with a summary of the error and with more details, if available.

The values in parentheses serve as placeholders and should be replaced with specific, relevant values.
`...` signifies that more arguments are expected.

# 3. Build infrastructure

The team should now start to build the virtual network and machine infrastructure.

All the machines should be named according to the company name and your team identity.

The planned infrastructure will need to have a set of separate (virtual) machines, with network isolation, namely:

- one database server;
- one application server, exposing an application programming interface (API);
- a client machine, to access the application server.

If the client machine only requires a web browser, it can be replaced by a machine outside the virtual network, such as the host machine, for example.

The database will need to store data for, at least, the documents.

The application server will need to access the database and perform CRUD operations (Create, Read, Update, Delete) for, at least, the documents.
These operations must be made available to clients, providing an application programming interface (API).

The choices of technologies to use will be entirely made by the project team.
We recommend that you use technologies that you are familiar with, from previous courses, but it is not mandatory.

This infrastructure should be presented to the teacher in the labs session, to receive feedback.

# 4. Protect server communications

Now that your team has an application infrastructure, it will have to make the communication with servers secure.

The team will have to search in the documentation for the chosen application and database servers how to configure the available secure channel technologies.

All the necessary keys will need to be generated, certified, and distributed in a documented way.

If the chosen servers do not support the configuration of secure communication protocols, the group will have to follow a secure tunnel approach with SSH or similar protocol/tool.

The team should verify the protection of the communication channels using tools that allow traffic interception and inspection, to make due dilligence to assure that encryption is actually being used.
This traffic analysis does not need to perform cryptanalysis.

All the configuration (and references) must be documented and the results should be presented to the lab teacher, again, to receive feedback.

# 5. Respond to security challenge

Now that your team has an application infrastructure with all server communications secure, with the library for protecting the documents, you are ready to face a security challenge.

The business scenario description will present two challenges as a set of new requirements. 
**Pick just one of the challenges.**
It is up to you to design a solution for it, extending or adapting what you have already developed.
The solution will probably need to perform key distribution in a dynamic way.

After having a topic assigned, your team should design a security solution.
The design must be written in a document.

You can bring a draft of the proposal to your lab session or office hours to present it and receive feedback.  
It is highly recommended that the proposal is reviewed by the lab teacher before implementation begins.

Plan a gradual implementation of the solution.
First, develop the more basic components and test them.
Then, move to the more advanced components.

Attend the lab sessions to present your ongoing development and receive feedback from the teacher.

# 6. Prepare demonstration

Once a stable and mostly complete solution is achieved, your team should start preparing the demonstration.
The focus of the presentation should be on the back-end of security, i.e., to show the security mechanisms at work;
it should NOT be on the front-end of the application.
It is very important that the messages and its payloads are clearly presented, with trace logs.

Also at this stage, write a `README` file with step-by-step instructions on how to run your project, following a template that will be provided.  
It is also good to record screenshots, screencasts or similar artifacts showing your project in action.

# 7. Complete report

The project report should be written as the project moves forward.
These draft versions of the report are useful for presenting the state of the project and getting feedback at each lab session.

The report must include the following elements:
 - Cover: with the ID of the group (e.g., T95), the name and istID of every member of the group, and the chosen scenario;
 - Index;
 - Introduction: specifying the security challenge that was chosen;
 - Developed solution: with system model, assumptions, threat model, system architecture and description of the protocols (use the appropriate diagrams for this ([1](https://guides.visual-paradigm.com/modeling-a-distributed-system-using-deployment-diagram/), [2](https://medium.com/@apicraft/demystifying-system-design-diagrams-a-comprehensive-guide-e58255187a3d)). Recommended tools: [SequenceDiagram](https://sequencediagram.org), [drawio](https://www.drawio.com), [Excalidraw](https://excalidraw.com));
 - Conclusion: detailing which requirements that were achieved and which weren't (use the requirement identifier).

# 8. Submit code and report

Before the final deadline of the project, submit all the developed code and resources, and the revised report, to reflect the final version of the work:
  - make sure you have pushed all the changes in the code, `REPORT.md` and `README.md` files to the Git repository;
  - create a release tag called `ready`.

# 9. Check presentation session

The project presentations will occur on the dates following the submission deadline.  
The presentation session calendar will be made available during the final days of the project.

Each team is assigned to a session, in their respective campus, preferably in the time-slot of laboratory they are enrolled to.  
If your group requires a change in the proposed slot, please contact faculty in advance.

Each group will have to attend the full presentation session where they will present and one additional session.  
The presentation slides must written in English and the presenters should talk in English.  
For the discussion questions, students can answer in Portuguese.

The presentations are done by the whole group, in person, on campus.
Each group must attend from the beginning of their assigned session and remain for the duration of it.

# 10. Present and defend

The presentation should be organized as follows:

- 8 minutes for presentation of the work done, supported by slides, including a mandatory 2 minute live demo  
(The live demonstration has specific evaluation weight. Also prepare a video of the demo, as rehearsal and backup, just in case there is a technical problem.);
- 7 minutes for questions and answers.

It is highly recommended that each presentation includes slides detailing:

- the secure document format;
- the built infrastructure;
- the configured secure channels and key distribution;
- the security challenge and how it was handled;
- main results and conclusions.

Each group member must participate and talk in the presentation, and be prepared to answer individual questions.  
If necessary, a more detailed discussion will be scheduled with each group.

Until the end of the day of your presentation session:
  - add presentation slides to the Git repository (both `PRESENTATION.pptx` and `PRESENTATION.pdf`);
  - add a `VIDEO.url` text file with a link for downloading the demonstration video file. The link should be valid for at least 7 days;
  - create a release tag called `done`.

----

For any questions, please contact:  
[SIRS Faculty](mailto:meic-sirs@disciplinas.tecnico.ulisboa.pt)
