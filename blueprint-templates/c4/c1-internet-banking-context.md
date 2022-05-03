---
layout: default
title: Internet Banking System Context
parent: C4 Blueprints
grand_parent: Blueprint Templates
nav_order: 20
---

# System Context diagram

A System Context diagram is a good starting point for diagramming and documenting a software system, allowing you to step back and see the big picture. Draw a diagram showing your system as a box in the centre, surrounded by its users and the other systems that it interacts with.

Detail isn't important here as this is your zoomed out view showing a big picture of the system landscape. The focus should be on people (actors, roles, personas, etc) and software systems rather than technologies, protocols and other low-level details. It's the sort of diagram that you could show to non-technical people.

Scope: A single software system.

Primary elements: The software system in scope.
Supporting elements: People (e.g. users, actors, roles, or personas) and software systems (external dependencies) that are directly connected to the software system in scope. Typically these other software systems sit outside the scope or boundary of your own software system, and you donât have responsibility or ownership of them.

Intended audience: Everybody, both technical and non-technical people, inside and outside of the software development team.

## Example

This is an example System Context diagram for a fictional Internet Banking System owned by Big Bank plc. It shows the people who use it, and the other software systems that the Internet Banking System has a relationship with. Personal Customers of the bank use the Internet Banking System to view information about their bank accounts, and to make payments. The Internet Banking System itself uses the bank's existing Mainframe Banking System to do this, and uses the bank's existing E-mail System to send e-mails to customers. A colour coding has been used to indicate which software systems exist already (the grey boxes).

{% plantuml %}

title Internet Banking System - System Context

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

Enterprise_Boundary(enterprise, "Big Bank plc") {
  System(MainframeBankingSystem, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.", $tags="Element+Software System+Existing System")
  System(EmailSystem, "E-mail System", "The internal Microsoft Exchange e-mail system.", $tags="Element+Software System+Existing System")
  System(InternetBankingSystem, "Internet Banking System", "Allows customers to view information about their bank accounts, and make payments.", $tags="Element+Software System")
}

Person_Ext(PersonalBankingCustomer, "Personal Banking Customer", "A customer of the bank, with personal bank accounts.", $tags="Element+Person+Customer")

Rel_D(PersonalBankingCustomer, InternetBankingSystem, "Views account balances, and makes payments using", $tags="Relationship")
Rel_D(InternetBankingSystem, MainframeBankingSystem, "Gets account information from, and makes payments using", $tags="Relationship")
Rel_D(InternetBankingSystem, EmailSystem, "Sends e-mail using", $tags="Relationship")
Rel_D(EmailSystem, PersonalBankingCustomer, "Sends e-mails to", $tags="Relationship")

SHOW_LEGEND()

{% endplantuml %}
