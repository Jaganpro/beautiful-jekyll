---
layout: post
title: Lightning Flow Best Practices
subtitle: Flow 10 Commandments
tags: [Lightning Flow, Salesforce, Low Code Tools]
comments: true
nav-short: true
readtime: true
---

## Flow Best Practices

### Rule 1: Flow Layouts (Winter '21 Release and Beyond)

* Starting with Winter '21 - It is recommended to start using `Auto Layout` option when building Flow Solutions.
* This option provides better readability and organization for everyone.

<p align="center">
  <img width="500" src="https://user-images.githubusercontent.com/2145211/93279293-e6587a00-f794-11ea-8642-1713cea04d03.png">
</p>

### Rule 2: Storing Records in Flows

* For most of our use cases, `Automatically Store all Fields` option is recommended. 
  * Salesforce does lot of optimization behind the scenes. So go ahead and use them without any performance impact.
* In rare cases, we have to manually Assign Variables. This option in my opinion exist only for backward compatibility.

<p align="center">
  <img width="500" src="https://user-images.githubusercontent.com/2145211/93279426-38010480-f795-11ea-992e-f5e14d8d23db.png">
</p>

### Rule 3: Consider Using Fault Connectors

* For any DML Operations, please consider using Fault Connectors.
* These Fault Connectors should be connected to Custom Exception Log Object (If Applicable or Available in your Salesforce Org). 
  * This may also need a Reusable Apex Action and will need be created to be used by all Configurators and Developers.
* Fault messages should be logged using available System Variables `{!$Flow.FaultMessage}`

<p align="center">
  <img width="500" src="https://user-images.githubusercontent.com/2145211/93279335-fc663a80-f794-11ea-8016-0050200b87e0.png">
</p>

* Fault Connectors are applicable for both `ScreenFlow` and `AutoLaunch Flows`.
* For `ScreenFlows` - It is expected to provide meaningful Error Message to EndUsers using Screen Components + log the bug using Apex Action.
* For `AutoLaunch` and `Scheduled` Flows - It is expected to log the bug using Apex Action.

### Rule 4: One Flow Per Object & Sub Flows Usage (Opinionated)

* Given that there are many No-Code Tool options available in Salesforce Platform, we want to design our flows to be manageable in future.
* For most of our use cases, try designing `One Flow Per Object` Rule (Wherever Applicable). 
  * Although, i see there could be some exceptions to this rule due to the need for Before and Afte Launch Flows because of business requirements.
  * What i am getting at is - try to design a flow using `DRY Principles (Do Not Repeat Yourself).
    * There is no point in creating 2 BeforeRun flows for a same sObject for a same operation (Say Created)
* When using One Flow per Object, we can also use `Sub Flows` to extend these flows for better organization.
  * As a rule of thumb, if a flow contains more than 25 elements, then it is time to think about segmenting business process into Sub-Flow.
* One Flow per Object only applies to `AutoLaunch Flows`. It does not apply to `Screen Flows` and `Scheduled Flows`.

### Rule 5: Flow Naming convention (Opinionated)

* Naming Convention Format
  * `<ObjectName>_<FlowType>_Run<Before(or)After>_<Created(And/Or)Updated>_<FlowLevel>_<BusinessProcessName>`

  * `<ObjectType>` - Name of sObject in Salesforce
  * `<FlowType>` - Choose between `AutoLaunch`, `Screen`, `Scheduled`
  * `Run<Before(or)After>` - Represents when Flow runs in given execution context.
  * `<Created(And/Or)Updated>` - Represents when Flow runs in given execution context.
  * `<FlowLevel>` - MainFlow (or) SubFlow

* Example Flow Name: `Account_AutoLaunch_RunBefore_CreatedAndUpdated_Main_UpdateRole`
* Sometimes, it is necessary to create multiple flows on the same object due to the nature of business requirements and thats ok.

### Rule 6: Flow Trigger Configuration

* It is preferred to use `Before Record Save Flow` for all Assignment requirements as it is 10X faster than next LowCode Solution available in Salesforce Platform.

<p align="center">
  <img width="500" src="https://user-images.githubusercontent.com/2145211/93279716-f02ead00-f795-11ea-91e2-4b8d9aad6d09.png">
</p>

* If DML Operations are necessary on sObject, then `After Save Flows` can be used.

### Rule 7: When to use Lightning Flows vs Code?

* Please refer to this document to determine when to use flows and when it is applicable to switch to Apex Code.
  * [Salesforce Architect Guide for Record Trigger Automation](https://salesforce.quip.com/VJfCAFhEBO0W)

<p align="center">
  <img width="500" src="https://user-images.githubusercontent.com/2145211/93279992-9bd7fd00-f796-11ea-8c80-b1c02491099d.png">
</p>

* This document is prepared by Salesforce Platform Team.

### Rule 8: Automation Components

* Flows are even more powerful when they are extended using `Automation Components`
* Automation Components are now officially adopted and supported by Salesforce Platform.
  * [TrailheadApps - Automation Components](https://github.com/trailheadapps/automation-components)
* Before building custom solutions or components in flow, please check for any existing solutions from community.
  * [Unofficial SF - Flow Actions](https://unofficialsf.com/flow-actions/)
  * [Unofficial SF - Flow Screen Components](https://unofficialsf.com/flow-screen-components/)

### Rule 9: Bulkify Flows

* Wherever possible, please use `Record Collection Variables` to insert or update data within Flows Loops
* Please don't perform DML or CRUD Operations within Loops
* Please don't use `Legacy Apex Actions` as they are not bulkified.

### Rule 10: Provide Meaningful Names for Assignments, Flows, Loops, Variables, Decisions etc.

* In this example below, you can see Assignment variables are meaningful to be easily understood by anyone.

<p align="center">
  <img width="350" src="https://user-images.githubusercontent.com/2145211/93280209-0f7a0a00-f797-11ea-9a7b-b85687638add.png">
</p>
