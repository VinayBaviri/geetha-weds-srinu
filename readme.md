
# EDGE NODE LAYER

A Node.js middle layer also known as Backend-for-frontend to handle all the API requests to fetch and manage data built on express framework.

[![Project Version](https://img.shields.io/badge/Project%20Version-0.6.0-brightgreen)](https://gl20200905.karkinos.in/edge-node-layer/edge-node/-/tree/master) [![Node Version](https://img.shields.io/badge/Node%20Version-14.17.3-brightgreen)](https://nodejs.org/download/release/v14.17.3/)  [![NPM Version](https://img.shields.io/badge/NPM%20Version-6.14.13-brightgreen)](https://nodejs.org/download/release/v14.17.3/)

# Getting Started
To get started, need to install Node version [14.17.3](https://nodejs.org/download/release/v14.17.3/ "14.17.3").
Look into the "scripts" of the `package.json` file located in the project root directory and choose your environment command to run.

Supported commands :

```sh
 npm run dev
```
```sh
 npm run sit
```
```sh
 npm run qa
```
```sh
 npm run demo
```
```sh
 npm run live
```

# Documentation

- [Introduction](#Introduction)
- [Folder-Structuring ](#Folder-Structuring)
- [Caching](#Caching)
- [DialogFlow](#DialogFlow)
- [Debugging](#Debugging)


### Introduction

Edge Node Layer acts as a middle layer between front-end applications and microservices.
It will also manages and modifies request data and responses as required by the front-end applications and micro-services.

### Folder-Structuring

- [common](#common)
- [config](#config)
- [controller](#controller)
- [database](#database)
- [environments](#environments)
- [logs](#logs)
- [model](#model)
- [node_modules](#node_modules)
- [routes](#routes)
- [root-files](#root-files)

### common

Consist of common
- redis-key expiration timer constants.
- error codes and its descriptive messages.
- functions responsible for setting headers and sending successful or error responses.

### config

Consist of configurations of
- dialogflow connection files.
- loging mechanism.
- redis connection.

### controller

Consist of controlling part of
- auto suggestions posting and retrieving logics from edge db.
- common business logics to control and modify data.
- dialog flow communication with node layer.
- user preferences posting and retrieving logics from edge db.
- user id and token posting and retrieving logics from edge db.

### database

- Connection configuration to edge db.
- Consist of posting and retrieving ( CRUD ) methods of
 - add on attributes into the edge db.
 - auto suggestions into the edge db.
 - constants into the edge db.
 - master data into the edge db.
 - user preferences into the edge db.
 - user data into the edge db.

### environments

Consist of environment variables for
- DEV
- SIT
- QA
- DEMO
- PRODUCTION

### logs

- Consist of project logger files.

### model

Consist of mongo db schema files of
- add on attributes.
- auto suggestions.
- constants.
- master data.
- user preferences.
- users.

### node_modules

- Consist of project dependency library files.
  - Check here to know more about project dependency [public libraries](https://docs.google.com/spreadsheets/d/1mCmZ9C-0wqz_pIfVMnBaB2jSzQFnVDCGiygff_pnSEE/edit#gid=939515795 "public libraries").

### routes

Consist of
- common data manage and data manipulation express route requests.
- node layer handling route requests.
- all master service end points.
- all patient service end points.
- common utils request end points.

Note : Each file ends with common http ( GET, POST, PUT, DELETE ) method routes to redirect request to micro-services without changing the req data. Also passes the response from micro-services to requested applications without any interruptions.

### root-files

- Consist of `.gitignore` file to ignore files or folders into the GIT.
- Consist of `gitpod.Dockerfile` file to customize GIT-POD workspace.
- Consist of `gitpod.yml` file to customize GIT-POD workspace.
- Consist of `package-lock.json` file to keep track of the exact version of every public package that is installed on the project.
- Consist of `package.json` file to keep project metadata, scripts, dependencies and dev-dependencies.
- Consist of `server.js` file which consist of project initiation code, routing paths, initiating connections with **LOGGER**, **REDIS** and **MONGO DB**.


### Caching
- In Edge node layer, using `REDIS` in-memory caching mechanism.

  Check below to get better understanding of what we are caching and its expiration timers

  **Description** - **Timer constant** - **Key**
  - Patient List - `one hour` -  `patientList`
  - Patient Data - `one hour ` - `patient_<patientId>`
  - Add on attributes - one day - `addOnAttributes`
  - Constants - `one hour` - `constant_<constantName>`
  - User info by userId - `one hour` - `user_<userId>`
  - Cancer types by site - `three days` - `cancerTypesBySite`
  - Medical info of one patient - `one hour` - `medicalInfo_<patientId>`
  - One patient photo - `one hour` - `patientPhoto_<patientId>`
  - One patient personal info - `one hour` - `personalInfo_<patientId>`
  - One patient status - `one hour` - `patientStatus_<patientId>`
  - Sample collection hash map - `one hour` - `SAMPLE_COLLECTION_map`
  - Master key VTB CONSENT - `one hour` - `VTB_CONSENT`
  - Master key FOLLOW UP CONSENT - `one hour` - `FOLLOW_UP_CONSENT`
  - Master key SAMPLE COLLECTION - `one hour` - `SAMPLE_COLLECTION`
  - Master key REGISTRATION POLICY - `one hour` - `REGISTRATION_POLICY`
  - Master key REGISTRATION CONSENT - `one hour` - `REGISTRATION_CONSENT`
  - Master key SCREENIGN CONSENT - `one hour` - `SCREENING_CONSENT`
  - Master key EXPERT OPINION CONSENT - `one hour` - `EXPERT_OPINION_CONSENT`
  - Master key GP CONSULTATION CONSENT - `one hour` - `GP_CONSULTATION_CONSENT`
  - Master key TELECONSULTATION CONSENT - `one hour` - `TELECONSULTATION_CONSENT`
  - Master key ORAL CANCER SCREENING CONSENT - `one hour` - `ORAL_CANCER_SCREENING_CONSENT`
  - Master key DAY CARE CHEMOTHERAPY CONSENT - `one hour` - `DAYCARE_CHEMOTHERAPY_CONSENT`
  - Master key REGISTRATION TERMS & CONDTIONS - `one hour` - `REGISTRATION_TERMS_AND_CONDITIONS`
  - Ui templates response - `two hours` - `<templateName>_<gender>_<version>_<locale>_response `
  - Ui templates hashmap response  - `two hours` - `<templateName>_<gender>_<version>_<locale>_map`

### DialogFlow
- To initiate communication with dialog-flow, configured http POST endpoint `<BASEURL>/edge/ask-karkinos/` in the node layer.
- When request comes, it will communicate with respective configured dialogflow agent in the [cloud](https://dialogflow.cloud.google.com/ "cloud").
- When triggered question matches with the configured intents in the dialogflow, it fires the node layer webhook ( configured http POST endpoint ) in the fulfillment section.
- Node layer validates the request params from dialogflow, fetches the data from micro-services and sends back the response to requested applications.

### Debugging

- To start edge node layer in debug mode, run the command `npm run debug`.
	- It will start project in debug mode and opens secured web socket connection url with uniqueId.
- Open new browser tab and navigate to `devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&wss=` and paste the secured web socket connection url ( without https protocol).

Sample url :: 

```sh
devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&wss=9229-f82efa1f-6a07-4db6-9717-346137840b74.ws.kgp.intranet.karkinos.in/1b72edf8-4dcb-46c5-9264-1954108ba023
```
