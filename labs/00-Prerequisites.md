# Prerequisites

To work with the following Labs please make sure that you have the following Accounts ready:

## Free Azure DevOps Tenant ---> [Register here](https://azure.microsoft.com/de-de/services/devops/)

## MyShuttle Demo Projekt

1. To Setup up the MyShuttle Demo Projekt in your Environment go to the [Azure DevOps Demo Generator](https://aka.ms/devopsdemogenerator)
2. Sign in with your Azure DevOps Credentials
3. Select MyShuttle App as template
4. Click "Create Project"
5. Got to your Azure DevOps Project after completion

## Azure Web App

Stack: Tomcat 9 - Java 11

Operating System: Windows

Region: WestEurope

Plan: S1

## Azure MySQL Database

Azure Database for MySQL

Version: 5.7

Wait for done

Settings --> Connection Security ---> Allow Access to Azure Services ON

Settings --> Properties

Copy ServerName and LogOnName to local Editor Window


jdbc:mysql://{MySQL Server Name}:3306/alm?useSSL=true&requireSSL=false&autoReconnect=true&user={your user name}&password={your password}

Replace with own settings

## Deployment Slots for Azure WebApp

New Connection String --> Name: MyShuttleDb Value: PASTE string from above

New Slot: Staging (inherit from Prod)



## Optional Container Registry

