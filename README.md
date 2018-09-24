# OSS PaaS and DevOps workshop

This repository contains resources for use with the OSS PaaS and DevOps workshop, including the starter MERN app, code for Azure Functions, scripts for seeding data into the MongoDB database, as well as exporting data, and an ARM template for deploying the Lab virtual machine (VM) to Azure.


## Agenda
* 09:00 - 09:30   Welcome and Introduction
* 09:30 - 10:00   Open Source in Microsoft
* 10:00 - 10:30   Intro to App Modernization
* 10:30 - 10:45   Coffee BreakHands on component
* 10:45 - 12:00   Containers & DevOps ([Whiteboard Session workshop](https://github.com/fxkim/MCW-OSS-PaaS-and-DevOps/tree/master/Whiteboard%20Design%20Session))
* 12:00 - 13:00   Lunch break
* 13:00 - 15:00   Hands-on Labs: [App Modernization](https://github.com/fxkim/MCW-OSS-PaaS-and-DevOps/tree/master/Hands-on%20Lab)
* 15:00 - 16:00   Hands-on Labs: App Modernization with CI/CD
* 16:00 - 16:30   Closing and Q&A

## Setup of Azure Subscription

To work with Azure you need an Azure Subscription. See the following guide to initialise a trial Azure Subscription: [Azure Pass](https://github.com/fxkim/MCW-OSS-PaaS-and-DevOps/blob/master/DevtoolsVMs/How%20to%20activate%20an%20Azure%20Pass%20Vouchers.pdf)

## Devtool VM

To get started, click the Deploy to Azure link below. This will provision a fully configured Linux or Windows Lab VM, used as a development machine for the OSS PaaS and DevOps workshop.

# Linux VM details:
1. Ubuntu Server 16.04 LTS / Windows Server 2016
2. Docker Community Edition
3. Visual Studio Code
4. Node.js and NPM
5. Mongo DB Community Edition
6. Additionnal tools: Terminator, Fish
7. Opens port 3389 to allow RDP connections

See the setup script: [LinuxDevtoolsVMSetup.sh](https://raw.githubusercontent.com/fxkim/MCW-OSS-PaaS-and-DevOps/master/DevtoolsVMs/LinuxDevtoolsVMSetup.sh)

[![Deploy Linux Devtools VM to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ffxkim%2FMCW-OSS-PaaS-and-DevOps%2Fmaster%2FDevtoolsVMs%2Fazure-deploy.json)


# Windows VM Details

1. Windows Server 2016
2. Docker Community Edition
3. Visual Studio Code
4. Node.js and NPM
5. Mongo DB Community Edition
6. Opens port 3389 to allow RDP connections

See the setup script: [WinDevToolsSetup.cmd](https://raw.githubusercontent.com/fxkim/MCW-OSS-PaaS-and-DevOps/master/DevtoolsVMs/WinDevToolsSetup.cmd)

[![Deploy Windows Devtools VM to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ffxkim%2FMCW-OSS-PaaS-and-DevOps%2Fmaster%2FDevtoolsVMs%2Fazure-deploy.json)

