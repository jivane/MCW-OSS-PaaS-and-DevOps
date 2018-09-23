# OSS PaaS and DevOps workshop

This repository contains resources for use with the OSS PaaS and DevOps workshop, including the starter MERN app, code for Azure Functions, scripts for seeding data into the MongoDB database, as well as exporting data, and an ARM template for deploying the Lab virtual machine (VM) to Azure.


## Agenda
* 09:00 - 09:30   Welcome and Introduction
* 09:30 - 10:00   Open Source in Microsoft
* 10:00 - 10:30   Intro to App Modernization
* 10:30 - 10:45   Coffee BreakHands on component
* 10:45 - 12:00   Containers & DevOps (Whiteboard Session workshop)
* 12:00 - 13:00   Lunch break
* 13:00 - 15:00   Hands-on Labs: App Modernization
* 15:00 - 16:00   Hands-on Labs: App Modernization with CI/CD
* 16:00 - 16:30   Closing and Q&A


## Devtool VM

To get started, click the Deploy to Azure link below. This will provision a fully configured Linux Lab VM, used as a development machine for the OSS PaaS and DevOps workshop.

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ffxkim%2FMCW-OSS-PaaS-and-DevOps%2Fmaster%2FDevtoolsVMs%2Fazure-deploy.json)

VM details:
1. Ubuntu Server 16.04 LTS
2. Docker Community Edition
3. Visual Studio Code
4. Node.js and NPM
5. Mongo DB Community Edition
6. Additionnal tools: Terminator, Fish
7. Opens port 3389 to allow RDP connections

See the setup script: [LinuxDevtoolsVMSetup.sh](https://raw.githubusercontent.com/fxkim/MCW-OSS-PaaS-and-DevOps/master/DevtoolsVMs/LinuxDevtoolsVMSetup.sh)

## Starter app

Once the VM is deployed, connect to it using an RDP client, fork the starter application into your own GitHub repo, and clone it to the Lab VM.

The starter project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

In the project directory, run:

```sh
npm install
node data/Seed.js
npm run build
npm start
```

### `npm install`

Installs required components.

### `node data/Seed.js`

Seeds the local MondoDB database with sample data.

### `npm run build`

Builds the app for production to the `build` folder. It correctly bundles React in production mode and optimizes the build for the best performance. The build is minified and the filenames include the hashes.

Your app is ready to be deployed!

### `npm start`

Runs the app in the development mode. You will also see any lint errors in the console.

Open [http://localhost:3000](http://localhost:3000) to view it in the browser.