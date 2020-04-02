# Arkyn Time - Realm Integration
This repository contains information about the iOS-app Arkyn Time and how to write an integration for a custom backend through the synced Realm-database on the device.
## Overview
Arkyn Time is the first app from Arkyn Studios - a startup focused on creating "consumer grade" apps within the Smart Enterprise space i.e. apps built for large coorporations (primarily with SAP backends).
## System Design
The system has been designed with a couple goals in mind:
- The device must not be waiting on the backend for long-running queries, commands, etc. All actions on the device must be close instantaneous in order to provide a responsive interface.
- It should be easy to write integrations to other backends than SAP.
- When using SAP backends the system should be easy to configure and get up and running quick - without the need for extensive integration projects.

To accomodate these goals Realm is used as the database on the device and Realm Cloud to keep the on-device database synced with the backend.

Apart from Realm a service for onboarding users and authentication is provided by Arkyn Studios.
## Data Model
Most time registration systems are built around the same concept: employees make a registration on a day - either with a start- and end-time or with a fixed amount of time. Then they choose what the registration is related to - that could be a project, an order number, etc. We call this part of the registration `reporting`.

Since different time registration systems have slight variances in how this is done the data model for the app is designed in a backend-agnostic manner with the notion of `reporting trees`. Examples of such reporting trees could be:
- Order No.
- Work breakdown structures (WBS)
- Customers
- Project
- Phases
- Activities
- Kind - Billable, not billable, overtime, etc.
- ... and many other

Many of these will only be lists (trees with depth 1), but they can have any depth required.

A `reporting` is then defined as a selection of `reporting tree nodes` identifying which elements from each tree to register the entry on. An entry can be created based on a `reporting template` that defines which trees to use in a registration. That is done by defining a list of `reporting contexts`. Examples of reporting templates could be: "Project", "Absence", "Leave".

![Example](Example1.png "Example")

In the above example the template defines two reporting contexts: WBS-number and activity. The finished reporting on the time entry is (in this case) `[947588800, 2115]` (Please note that in practice these will be uuids identifying each node and not their backend IDs as shown here fore the sake of simplicity).

The backend integration must provide these trees, contexts, sections and templates in the device database - preferable tailored to the individual user to create a better user experience.

### TimeEntry
The main class of the application containing a single time entry.
#### Properties
- id: (string, required) - A uuid for this specific time entry
...

### ReportingTemplate
Class providing a templates for different kinds of registrations.
####

## Integration
### Authentication
### Backend


Spørgsmål: 
- Kan man forestille sig at et træ er mandatory i en context, men ikke i en anden?
- Kan man vælge mere end en node fra samme træ?