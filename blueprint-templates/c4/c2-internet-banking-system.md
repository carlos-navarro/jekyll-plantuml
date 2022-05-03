---
layout: default
title: Internet Banking System
parent: C4 Blueprints
grand_parent: Blueprint Templates
nav_order: 30
---

# Container diagram

Once you understand how your system fits in to the overall IT environment, a really useful next step is to zoom-in to the system boundary with a Container diagram. A "container" is something like a server-side web application, single-page application, desktop application, mobile app, database schema, file system, etc. Essentially, a container is a separately runnable/deployable unit (e.g. a separate process space) that executes code or stores data.

The Container diagram shows the high-level shape of the software architecture and how responsibilities are distributed across it. It also shows the major technology choices and how the containers communicate with one another. It's a simple, high-level technology focussed diagram that is useful for software developers and support/operations staff alike.

Scope: A single software system.

Primary elements: Containers within the software system in scope.
Supporting elements: People and software systems directly connected to the containers.

Intended audience: Technical people inside and outside of the software development team; including software architects, developers and operations/support staff.

Notes: This diagram says nothing about deployment scenarios, clustering, replication, failover, etc.

## Example

This is an example Container diagram for a fictional Internet Banking System. It shows that the Internet Banking System (the dashed box) is made up of five containers: a server-side Web Application, a Single-Page Application, a Mobile App, a server-side API Application, and a Database. The Web Application is a Java/Spring MVC web application that simply serves static content (HTML, CSS and JavaScript), including the content that makes up the Single-Page Application. The Single-Page Application is an Angular application that runs in the customer's web browser, providing all of the Internet banking features. Alternatively, customers can use the cross-platform Xamarin Mobile App, to access a subset of the Internet banking functionality.

Both the Single-Page Application and Mobile App use a JSON/HTTPS API, which is provided by another Java/Spring MVC application running on the server. The API Application gets user information from the Database (a relational database schema). The API Application also communicates with the existing Mainframe Banking System, using a proprietary XML/HTTPS interface, to get information about bank accounts or make transactions. The API Application also uses the existing E-mail System if it needs to send e-mails to customers.

The dashed line represents the boundary of the Internet Banking System, showing the containers (light blue) inside it.

{% plantuml %}

title Internet Banking System - Containers

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

Person_Ext(PersonalBankingCustomer, "Personal Banking Customer", "A customer of the bank, with personal bank accounts.", $tags="Element+Person+Customer")
System(MainframeBankingSystem, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.", $tags="Element+Software System+Existing System")
System(EmailSystem, "E-mail System", "The internal Microsoft Exchange e-mail system.", $tags="Element+Software System+Existing System")

System_Boundary("InternetBankingSystem_boundary", "Internet Banking System") {
  Container(InternetBankingSystem.WebApplication, "Web Application", "Java and Spring MVC", "Delivers the static content and the Internet banking single page application.", $tags="Element+Container")
  Container(InternetBankingSystem.APIApplication, "API Application", "Java and Spring MVC", "Provides Internet banking functionality via a JSON/HTTPS API.", $tags="Element+Container")
  ContainerDb(InternetBankingSystem.Database, "Database", "Oracle Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.", $tags="Element+Container+Database")
  Container(InternetBankingSystem.SinglePageApplication, "Single-Page Application", "JavaScript and Angular", "Provides all of the Internet banking functionality to customers via their web browser.", $tags="Element+Container+Web Browser")
  Container(InternetBankingSystem.MobileApp, "Mobile App", "Xamarin", "Provides a limited subset of the Internet banking functionality to customers via their mobile device.", $tags="Element+Container+Mobile App")
}

Rel_D(EmailSystem, PersonalBankingCustomer, "Sends e-mails to", $tags="Relationship")
Rel_D(PersonalBankingCustomer, InternetBankingSystem.WebApplication, "Visits bigbank.com/ib using", "HTTPS", $tags="Relationship")
Rel_D(PersonalBankingCustomer, InternetBankingSystem.SinglePageApplication, "Views account balances, and makes payments using", $tags="Relationship")
Rel_D(PersonalBankingCustomer, InternetBankingSystem.MobileApp, "Views account balances, and makes payments using", $tags="Relationship")
Rel_D(InternetBankingSystem.WebApplication, InternetBankingSystem.SinglePageApplication, "Delivers to the customer's web browser", $tags="Relationship")
Rel_D(InternetBankingSystem.SinglePageApplication, InternetBankingSystem.APIApplication, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.MobileApp, InternetBankingSystem.APIApplication, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication, InternetBankingSystem.Database, "Reads from and writes to", "JDBC", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication, MainframeBankingSystem, "Makes API calls to", "XML/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication, EmailSystem, "Sends e-mail using", $tags="Relationship")

SHOW_LEGEND()

{% endplantuml %}
