---
layout: default
title: System Landscape diagram
parent: C4 Blueprints
grand_parent: Blueprint Templates
nav_order: 10
---

# System Landscape diagram

The C4 model provides a static view of a single software system but, in the real-world, software systems never live in isolation. For this reason, and particularly if you are responsible for a collection of software systems, it's often useful to understand how all of these software systems fit together within the bounds of an enterprise. To do this, you can add another diagram that sits "on top" of the C4 diagrams, to show the system landscape from an IT perspective. Like the System Context diagram, this diagram can show organisational boundaries, internal/external users and internal/external systems.

Essentially this is a high-level map of the software systems at the enterprise level, with a C4 drill-down for each software system of interest. From a practical perspective, a system landscape diagram is really just a system context diagram without a specific focus on a particular software system.

Scope: An enterprise.

Primary elements: People and software systems related to the enterprise in scope.

Intended audience: Technical and non-technical people, inside and outside of the software development team.

## Example

Here is a System Landscape diagram from the "Big Bank plc" example workspace, showing all of the users and software systems defined within the workspace:

{% plantuml %}

title System Landscape for Big Bank plc

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

Enterprise_Boundary(enterprise, "Big Bank plc") {
  Person(CustomerServiceStaff, "Customer Service Staff", "Customer service staff within the bank.", $tags="Element+Person+Bank Staff")
  Person(BackOfficeStaff, "Back Office Staff", "Administration and support staff within the bank.", $tags="Element+Person+Bank Staff")
  System(MainframeBankingSystem, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.", $tags="Element+Software System+Existing System")
  System(EmailSystem, "E-mail System", "The internal Microsoft Exchange e-mail system.", $tags="Element+Software System+Existing System")
  System(ATM, "ATM", "Allows customers to withdraw cash.", $tags="Element+Software System+Existing System")
  System(InternetBankingSystem, "Internet Banking System", "Allows customers to view information about their bank accounts, and make payments.", $tags="Element+Software System")
}

Person_Ext(PersonalBankingCustomer, "Personal Banking Customer", "A customer of the bank, with personal bank accounts.", $tags="Element+Person+Customer")

Rel_D(PersonalBankingCustomer, InternetBankingSystem, "Views account balances, and makes payments using", $tags="Relationship")
Rel_D(InternetBankingSystem, MainframeBankingSystem, "Gets account information from, and makes payments using", $tags="Relationship")
Rel_D(InternetBankingSystem, EmailSystem, "Sends e-mail using", $tags="Relationship")
Rel_D(EmailSystem, PersonalBankingCustomer, "Sends e-mails to", $tags="Relationship")
Rel_D(PersonalBankingCustomer, CustomerServiceStaff, "Asks questions to", "Telephone", $tags="Relationship")
Rel_D(CustomerServiceStaff, MainframeBankingSystem, "Uses", $tags="Relationship")
Rel_D(PersonalBankingCustomer, ATM, "Withdraws cash using", $tags="Relationship")
Rel_D(ATM, MainframeBankingSystem, "Uses", $tags="Relationship")
Rel_D(BackOfficeStaff, MainframeBankingSystem, "Uses", $tags="Relationship")

SHOW_LEGEND()

{% endplantuml %}
