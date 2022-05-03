---
layout: default
title: API Application
parent: C4 Blueprints
grand_parent: Blueprint Templates
nav_order: 40
---

# Component diagram

Following on from a Container diagram, you can zoom in and decompose each container further to identify the major structural building blocks and their interactions.

The Component diagram shows how a container is made up of a number of "components", what each of those components are, their responsibilities and the technology/implementation details.

Scope: A single container.

Primary elements: Components within the container in scope.
Supporting elements: Containers (within the software system in scope) plus people and software systems directly connected to the components.

Intended audience: Software architects and developers.

## Example

This is an example Component diagram for a fictional Internet Banking System, showing some (rather than all) of the components within the API Application. Here, there are three Spring MVC Rest Controllers providing access points for the JSON/HTTPS API, with each controller subsequently using other components to access data from the Database and Mainframe Banking System, or send e-mails.

The dashed line represents the boundary of the API Application, showing the components (light blue) inside it.

{% plantuml %}

title Internet Banking System - API Application - Components

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

System(MainframeBankingSystem, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.", $tags="Element+Software System+Existing System")
System(EmailSystem, "E-mail System", "The internal Microsoft Exchange e-mail system.", $tags="Element+Software System+Existing System")
ContainerDb(InternetBankingSystem.Database, "Database", "Oracle Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.", $tags="Element+Container+Database")
Container(InternetBankingSystem.SinglePageApplication, "Single-Page Application", "JavaScript and Angular", "Provides all of the Internet banking functionality to customers via their web browser.", $tags="Element+Container+Web Browser")
Container(InternetBankingSystem.MobileApp, "Mobile App", "Xamarin", "Provides a limited subset of the Internet banking functionality to customers via their mobile device.", $tags="Element+Container+Mobile App")

Container_Boundary("InternetBankingSystem.APIApplication_boundary", "API Application") {
  Component(InternetBankingSystem.APIApplication.SignInController, "Sign In Controller", "Spring MVC Rest Controller", "Allows users to sign in to the Internet Banking System.", $tags="Element+Component")
  Component(InternetBankingSystem.APIApplication.AccountsSummaryController, "Accounts Summary Controller", "Spring MVC Rest Controller", "Provides customers with a summary of their bank accounts.", $tags="Element+Component")
  Component(InternetBankingSystem.APIApplication.ResetPasswordController, "Reset Password Controller", "Spring MVC Rest Controller", "Allows users to reset their passwords with a single use URL.", $tags="Element+Component")
  Component(InternetBankingSystem.APIApplication.SecurityComponent, "Security Component", "Spring Bean", "Provides functionality related to signing in, changing passwords, etc.", $tags="Element+Component")
  Component(InternetBankingSystem.APIApplication.MainframeBankingSystemFacade, "Mainframe Banking System Facade", "Spring Bean", "A facade onto the mainframe banking system.", $tags="Element+Component")
  Component(InternetBankingSystem.APIApplication.EmailComponent, "E-mail Component", "Spring Bean", "Sends e-mails to users.", $tags="Element+Component")
}

Rel_D(InternetBankingSystem.SinglePageApplication, InternetBankingSystem.APIApplication.SignInController, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.SinglePageApplication, InternetBankingSystem.APIApplication.AccountsSummaryController, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.SinglePageApplication, InternetBankingSystem.APIApplication.ResetPasswordController, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.MobileApp, InternetBankingSystem.APIApplication.SignInController, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.MobileApp, InternetBankingSystem.APIApplication.AccountsSummaryController, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.MobileApp, InternetBankingSystem.APIApplication.ResetPasswordController, "Makes API calls to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.SignInController, InternetBankingSystem.APIApplication.SecurityComponent, "Uses", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.AccountsSummaryController, InternetBankingSystem.APIApplication.MainframeBankingSystemFacade, "Uses", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.ResetPasswordController, InternetBankingSystem.APIApplication.SecurityComponent, "Uses", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.ResetPasswordController, InternetBankingSystem.APIApplication.EmailComponent, "Uses", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.SecurityComponent, InternetBankingSystem.Database, "Reads from and writes to", "JDBC", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.MainframeBankingSystemFacade, MainframeBankingSystem, "Makes API calls to", "XML/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.EmailComponent, EmailSystem, "Sends e-mail using", $tags="Relationship")

SHOW_LEGEND()

{% endplantuml %}
