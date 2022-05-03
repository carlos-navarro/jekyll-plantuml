---
layout: default
title: Deployment diagram
parent: C4 Blueprints
grand_parent: Blueprint Templates
nav_order: 100
---

# Deployment diagram

A deployment diagram allows you to illustrate how software systems and/or containers in the static model are mapped to infrastructure. This deployment diagram is based upon a UML deployment diagram, although simplified slightly to show the mapping between instances of software systems/containers and deployment nodes. A deployment node is something like physical infrastructure (e.g. a physical server or device), virtualised infrastructure (e.g. IaaS, PaaS, a virtual machine), containerised infrastructure (e.g. a Docker container), an execution environment (e.g. a database server, Java EE web/application server, Microsoft IIS), etc. Deployment nodes can be nested.

You may also want to include infrastructure nodes such as DNS services, load balancers, firewalls, etc.

Scope: One or more software systems.

Primary elements: Deployment nodes, software system instances, and container instances.
Supporting elements: Infrastructure nodes used in the deployment of the software system.

Intended audience: Technical people inside and outside of the software development team; including software architects, developers, infrastructure architects, and operations/support staff.

## Example

This is an example deployment diagram showing how the various containers of the Internet Banking System might be deployed in the bank's live environment:

{% plantuml %}

title Internet Banking System - Deployment - Development

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Deployment.puml

Deployment_Node(Development.DeveloperLaptop, "Developer Laptop", "Microsoft Windows 10 or Apple macOS", $tags="Element+Deployment Node") {
  Deployment_Node(Development.DeveloperLaptop.DockerContainerDatabaseServer, "Docker Container - Database Server", "Docker", $tags="Element+Deployment Node") {
    Deployment_Node(Development.DeveloperLaptop.DockerContainerDatabaseServer.DatabaseServer, "Database Server", "Oracle 12c", $tags="Element+Deployment Node") {
      ContainerDb(Development.DeveloperLaptop.DockerContainerDatabaseServer.DatabaseServer.Database_1, "Database", "Oracle Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.", $tags="Element+Container+Database+Container Instance")
    }

  }

  Deployment_Node(Development.DeveloperLaptop.DockerContainerWebServer, "Docker Container - Web Server", "Docker", $tags="Element+Deployment Node") {
    Deployment_Node(Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat, "Apache Tomcat", "Apache Tomcat 8.x", $tags="Element+Deployment Node") {
      Container(Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat.APIApplication_1, "API Application", "Java and Spring MVC", "Provides Internet banking functionality via a JSON/HTTPS API.", $tags="Element+Container+Container Instance")
      Container(Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat.WebApplication_1, "Web Application", "Java and Spring MVC", "Delivers the static content and the Internet banking single page application.", $tags="Element+Container+Container Instance")
    }

  }

  Deployment_Node(Development.DeveloperLaptop.WebBrowser, "Web Browser", "Chrome, Firefox, Safari, or Edge", $tags="Element+Deployment Node") {
    Container(Development.DeveloperLaptop.WebBrowser.SinglePageApplication_1, "Single-Page Application", "JavaScript and Angular", "Provides all of the Internet banking functionality to customers via their web browser.", $tags="Element+Container+Web Browser+Container Instance")
  }

}

Deployment_Node(Development.BigBankplc, "Big Bank plc", "Big Bank plc data center", $tags="Element+Deployment Node+") {
  Deployment_Node(Development.BigBankplc.bigbankdev001, "bigbank-dev001", $tags="Element+Deployment Node+") {
    System(Development.BigBankplc.bigbankdev001.MainframeBankingSystem_1, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.", $tags="Element+Software System+Existing System+Software System Instance")
  }

}

Rel_D(Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat.WebApplication_1, Development.DeveloperLaptop.WebBrowser.SinglePageApplication_1, "Delivers to the customer's web browser", $tags="Relationship")
Rel_D(Development.DeveloperLaptop.WebBrowser.SinglePageApplication_1, Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat.APIApplication_1, "Makes API calls to", "JSON/HTTPS", $tags="")
Rel_D(Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat.APIApplication_1, Development.DeveloperLaptop.DockerContainerDatabaseServer.DatabaseServer.Database_1, "Reads from and writes to", "JDBC", $tags="")
Rel_D(Development.DeveloperLaptop.DockerContainerWebServer.ApacheTomcat.APIApplication_1, Development.BigBankplc.bigbankdev001.MainframeBankingSystem_1, "Makes API calls to", "XML/HTTPS", $tags="")

SHOW_LEGEND()

{% endplantuml %}

{% plantuml %}

title Internet Banking System - Deployment - Live

top to bottom direction

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Deployment.puml

Deployment_Node(Live.Customersmobiledevice, "Customer's mobile device", "Apple iOS or Android", $tags="Element+Deployment Node") {
  Container(Live.Customersmobiledevice.MobileApp_1, "Mobile App", "Xamarin", "Provides a limited subset of the Internet banking functionality to customers via their mobile device.", $tags="Element+Container+Mobile App+Container Instance")
}

Deployment_Node(Live.Customerscomputer, "Customer's computer", "Microsoft Windows or Apple macOS", $tags="Element+Deployment Node") {
  Deployment_Node(Live.Customerscomputer.WebBrowser, "Web Browser", "Chrome, Firefox, Safari, or Edge", $tags="Element+Deployment Node") {
    Container(Live.Customerscomputer.WebBrowser.SinglePageApplication_1, "Single-Page Application", "JavaScript and Angular", "Provides all of the Internet banking functionality to customers via their web browser.", $tags="Element+Container+Web Browser+Container Instance")
  }

}

Deployment_Node(Live.BigBankplc, "Big Bank plc", "Big Bank plc data center", $tags="Element+Deployment Node") {
  Deployment_Node(Live.BigBankplc.bigbankapi, "bigbank-api*** (x8)", "Ubuntu 16.04 LTS", $tags="Element+Deployment Node+") {
    Deployment_Node(Live.BigBankplc.bigbankapi.ApacheTomcat, "Apache Tomcat", "Apache Tomcat 8.x", $tags="Element+Deployment Node") {
      Container(Live.BigBankplc.bigbankapi.ApacheTomcat.APIApplication_1, "API Application", "Java and Spring MVC", "Provides Internet banking functionality via a JSON/HTTPS API.", $tags="Element+Container+Container Instance")
    }

  }

  Deployment_Node(Live.BigBankplc.bigbankdb01, "bigbank-db01", "Ubuntu 16.04 LTS", $tags="Element+Deployment Node") {
    Deployment_Node(Live.BigBankplc.bigbankdb01.OraclePrimary, "Oracle - Primary", "Oracle 12c", $tags="Element+Deployment Node") {
      ContainerDb(Live.BigBankplc.bigbankdb01.OraclePrimary.Database_1, "Database", "Oracle Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.", $tags="Element+Container+Database+Container Instance")
    }

  }

  Deployment_Node(Live.BigBankplc.bigbankdb02, "bigbank-db02", "Ubuntu 16.04 LTS", $tags="Element+Deployment Node+Failover") {
    Deployment_Node(Live.BigBankplc.bigbankdb02.OracleSecondary, "Oracle - Secondary", "Oracle 12c", $tags="Element+Deployment Node+Failover") {
      ContainerDb(Live.BigBankplc.bigbankdb02.OracleSecondary.Database_1, "Database", "Oracle Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.", $tags="Element+Container+Database+Container Instance")
    }

  }

  Deployment_Node(Live.BigBankplc.bigbankprod001, "bigbank-prod001", $tags="Element+Deployment Node+") {
    System(Live.BigBankplc.bigbankprod001.MainframeBankingSystem_1, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.", $tags="Element+Software System+Existing System+Software System Instance")
  }

  Deployment_Node(Live.BigBankplc.bigbankweb, "bigbank-web*** (x4)", "Ubuntu 16.04 LTS", $tags="Element+Deployment Node+") {
    Deployment_Node(Live.BigBankplc.bigbankweb.ApacheTomcat, "Apache Tomcat", "Apache Tomcat 8.x", $tags="Element+Deployment Node") {
      Container(Live.BigBankplc.bigbankweb.ApacheTomcat.WebApplication_1, "Web Application", "Java and Spring MVC", "Delivers the static content and the Internet banking single page application.", $tags="Element+Container+Container Instance")
    }

  }

}

Rel_D(Live.BigBankplc.bigbankweb.ApacheTomcat.WebApplication_1, Live.Customerscomputer.WebBrowser.SinglePageApplication_1, "Delivers to the customer's web browser", $tags="Relationship")
Rel_D(Live.Customerscomputer.WebBrowser.SinglePageApplication_1, Live.BigBankplc.bigbankapi.ApacheTomcat.APIApplication_1, "Makes API calls to", "JSON/HTTPS", $tags="")
Rel_D(Live.Customersmobiledevice.MobileApp_1, Live.BigBankplc.bigbankapi.ApacheTomcat.APIApplication_1, "Makes API calls to", "JSON/HTTPS", $tags="")
Rel_D(Live.BigBankplc.bigbankapi.ApacheTomcat.APIApplication_1, Live.BigBankplc.bigbankdb01.OraclePrimary.Database_1, "Reads from and writes to", "JDBC", $tags="")
Rel_D(Live.BigBankplc.bigbankapi.ApacheTomcat.APIApplication_1, Live.BigBankplc.bigbankdb02.OracleSecondary.Database_1, "Reads from and writes to", "JDBC", $tags="")
Rel_D(Live.BigBankplc.bigbankapi.ApacheTomcat.APIApplication_1, Live.BigBankplc.bigbankprod001.MainframeBankingSystem_1, "Makes API calls to", "XML/HTTPS", $tags="")
Rel_D(Live.BigBankplc.bigbankdb01.OraclePrimary, Live.BigBankplc.bigbankdb02.OracleSecondary, "Replicates data to", $tags="Relationship")

SHOW_LEGEND()

{% endplantuml %}
