---
layout: default
title: API Sign In
parent: C4 Blueprints
grand_parent: Blueprint Templates
nav_order: 50
---

# Dynamic diagram

A dynamic diagram can be useful when you want to show how elements in a static model collaborate at runtime to implement a user story, use case, feature, etc. This dynamic diagram is based upon a UML communication diagram (previously known as a "UML collaboration diagram"). It is similar to a UML sequence diagram although it allows a free-form arrangement of diagram elements with numbered interactions to indicate ordering.

Scope: An enterprise, software system or container.

Primary and supporting elements: Depends on the diagram scope; enterprise (see System Landscape diagram), software system (see System Context or Container diagrams), container (see Component diagram).

Intended audience: Technical and non-technical people, inside and outside of the software development team.

## Example

This is an example dynamic diagram, showing how the "sign in" feature might work. The interactions can be animated, and the buttons are as follows:

{% plantuml %}

title API Application - Dynamic

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

Container_Boundary("InternetBankingSystem.APIApplication_boundary", "API Application") {
  Component(InternetBankingSystem.APIApplication.SignInController, "Sign In Controller", "Spring MVC Rest Controller", "Allows users to sign in to the Internet Banking System.", $tags="Element+Component")
  Component(InternetBankingSystem.APIApplication.SecurityComponent, "Security Component", "Spring Bean", "Provides functionality related to signing in, changing passwords, etc.", $tags="Element+Component")
}

ContainerDb(InternetBankingSystem.Database, "Database", "Oracle Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.", $tags="Element+Container+Database")
Container(InternetBankingSystem.SinglePageApplication, "Single-Page Application", "JavaScript and Angular", "Provides all of the Internet banking functionality to customers via their web browser.", $tags="Element+Container+Web Browser")

Rel_D(InternetBankingSystem.SinglePageApplication, InternetBankingSystem.APIApplication.SignInController, "1. Submits credentials to", "JSON/HTTPS", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.SignInController, InternetBankingSystem.APIApplication.SecurityComponent, "2. Validates credentials using", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.SecurityComponent, InternetBankingSystem.Database, "3. select * from users where username = ?", "JDBC", $tags="Relationship")
Rel_D(InternetBankingSystem.Database, InternetBankingSystem.APIApplication.SecurityComponent, "4. Returns user data to", "JDBC", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.SecurityComponent, InternetBankingSystem.APIApplication.SignInController, "5. Returns true if the hashed password matches", $tags="Relationship")
Rel_D(InternetBankingSystem.APIApplication.SignInController, InternetBankingSystem.SinglePageApplication, "6. Sends back an authentication token to", "JSON/HTTPS", $tags="Relationship")

SHOW_LEGEND()

{% endplantuml %}
