---
title: "Blueprint Example"
permalink: /docs/blueprint-example
excerpt: "A blueprint example that explore basic concepts of the generator."
last_modified_at: 2019-12-03T21:36:11-04:00
redirect_from:
  - /theme-setup/
toc: true
---


This document describes a blueprint example and is organized in the following steps:

1. Step 1: Design a BPMN Process
1. Step 2: Register a Process Definition
1. Step 3: Create a Domain Entity
1. Step 4: Create a Process Entity
1. Step 5: Create the Task Entities

## Step 1: Design a BPMN process

Create a process definition using the Camunda Modeler. In the blueprint example, we created a process called **Travel Plan Process** with three tasks: **Reserve a flight**, **Reserve a hotel** and **Reserve a car**.

![Image 1](https://user-images.githubusercontent.com/4369840/64934797-3ec00900-d81b-11e9-94ee-547eb9f60b92.png)

The process id will be used to create the process and task entities. The process should be marked as **Executable**.

<img width="1127" alt="Screen Shot 2019-09-16 at 00 44 35" src="https://user-images.githubusercontent.com/4369840/64934807-47184400-d81b-11e9-9729-11523dd78fa7.png">

The task id will be used to create the corresponding task entity. The tasks should be marked as **User Task**.

## Step 2: Register a Process Definition

In the web application, create a Process Definition using the menu Entities -> Process Definition.

<img width="1078" alt="Screen Shot 2019-09-16 at 00 56 21" src="https://user-images.githubusercontent.com/4369840/64935084-e984f700-d81c-11e9-8540-d9c4708fa6e9.png">

<img width="867" alt="Screen Shot 2019-09-16 at 00 56 28" src="https://user-images.githubusercontent.com/4369840/64935089-eee24180-d81c-11e9-83c9-c204320d526f.png">


## Step 3: Create a Domain Entity

Create an entity that represents the process domain. This entity will be handled by the tasks that compose the process.

In the blueprint example, we created a domain entity called **TravelPlanDomain** as showed in the image below:

![b5d4783e](https://user-images.githubusercontent.com/4369840/64935239-1be32400-d81e-11e9-9c32-843ef5813fdb.png)

To generate this entity, we created the file `.jhipster/TravelPlanDomain.json`:

    {
        "fluentMethods": true,
        "relationships": [],
        "fields": [
            {
                "fieldName": "startDate",
                "fieldType": "LocalDate"
            },
            {
                "fieldName": "endDate",
                "fieldType": "LocalDate"
            },
            {
                "fieldName": "flightBookingNumber",
                "fieldType": "String"
            },
            {
                "fieldName": "flightCompanyName",
                "fieldType": "String"
            },
            {
                "fieldName": "flightPrice",
                "fieldType": "BigDecimal"
            },
            {
                "fieldName": "hotelBookingNumber",
                "fieldType": "String"
            },
            {
                "fieldName": "hotelName",
                "fieldType": "String"
            },
            {
                "fieldName": "hotelPrice",
                "fieldType": "BigDecimal"
            },
            {
                "fieldName": "carBookingNumber",
                "fieldType": "String"
            },
            {
                "fieldName": "carCompanyName",
                "fieldType": "String"
            },
            {
                "fieldName": "carPrice",
                "fieldType": "BigDecimal"
            },
            {
                "fieldName": "finalPrice",
                "fieldType": "BigDecimal"
            },
            {
                "fieldName": "status",
                "fieldType": "String"
            }
        ],
        "changelogDate": "20190828214820",
        "dto": "mapstruct",
        "service": "serviceClass",
        "entityTableName": "travel_plan_domain",
        "jpaMetamodelFiltering": false,
        "pagination": "no"
    }

Execute the following command in the project root directory:    

    yo pa-jhipster:entity-domain TravelPlanDomain --regenerate

Say 'yes' to all the confirmation questions as indicated in the image below.

<img width="707" alt="Screen Shot 2019-09-16 at 01 17 26" src="https://user-images.githubusercontent.com/4369840/64935534-d4f62e00-d81f-11e9-80f9-f324cf500e42.png">


## Step 4: Create the Process Entity

Create an entity that represents the process instance. 

In the blueprint example, we created a process entity called **TravelPlanProcess**.

To generate this entity, we created the file `.jhipster/TravelPlanProcess.json`
    
    {
        "relationships": [],
        "fields": [
            {
                "fieldName": "startDate",
                "fieldType": "LocalDate"
            },
            {
                "fieldName": "endDate",
                "fieldType": "LocalDate"
            }
        ],
        "changelogDate": "20190828214820",
        "entityTableName": "travel_plan_process",
        "bpmnProcessDefinitionId": "TravelPlanProcess",
        "domainEntityType": "TravelPlanDomain",
        "jpaMetamodelFiltering": false,
        "pagination": "no"
    }

The `fields` list describes the fields that will be updated during the process instantiation.
Note that in this section you can only use fields that were declared in the domain entity. You cannot describe new fields here. 

The `bpmnProcessDefinitionId` is the process id that was defined in the bpmn file. 

The `domainEntityType` is the name of the domain entity (created in step 3).
  
Execute the following command in the project root directory:    

    yo pa-jhipster:entity-process TravelPlanProcess --regenerate

Say 'yes' to all the confirmation questions as indicated in the image below.

<img width="675" alt="Screen Shot 2019-09-16 at 01 23 38" src="https://user-images.githubusercontent.com/4369840/64935733-cc522780-d820-11e9-9395-aaca55165e04.png">

At this point, you are already able to create a process instance using the menu **Entities -> Travel Plan Process**.

<img width="1168" alt="Screen Shot 2019-09-16 at 01 41 02" src="https://user-images.githubusercontent.com/4369840/64936271-15a37680-d823-11e9-9385-471c871da5fb.png">

At the process instantiation, it possible to define a business key and the start and end dates.

<img width="912" alt="Screen Shot 2019-09-16 at 01 38 08" src="https://user-images.githubusercontent.com/4369840/64936283-1e944800-d823-11e9-9116-540a70d4464d.png">

After creating the process instance, it possible to see process instance, the domain instance, and the task.

In the Travel Plan Process page (accessed through the menu **Entities -> Travel Plan Process**), it is possible to see the process instance.

<img width="1169" alt="Screen Shot 2019-09-16 at 01 39 04" src="https://user-images.githubusercontent.com/4369840/64936293-248a2900-d823-11e9-826d-e210bcc8b890.png">

In the Travel Plan Domain page (accessed through the menu **Entities -> Travel Plan Domain**), it is possible to see the domain instance.

<img width="1157" alt="Screen Shot 2019-09-16 at 02 07 47" src="https://user-images.githubusercontent.com/4369840/64937100-e7279a80-d826-11e9-8d9f-4c3bf955b45d.png">

In the Task List page (accessed through the menu **Entities -> Tasks**), it is possible to see the first task of this process instance.

<img width="1170" alt="Screen Shot 2019-09-16 at 01 39 56" src="https://user-images.githubusercontent.com/4369840/64936301-2bb13700-d823-11e9-8c94-c356222a7aa3.png">


## Step 5: Create the Entities Task

The last step is to create the task entities. 

### Step 5.1: Create the Reserve a Flight Task

To generate the task **Reserve a flight**, we created the file `.jhipster/FlightTask.json`:

    {
        "relationships": [],
        "fields": [
            {
                "fieldName": "startDate",
                "fieldType": "LocalDate",
                "fieldReadOnly": true
            },
            {
                "fieldName": "endDate",
                "fieldType": "LocalDate",
                "fieldReadOnly": true
            },
            {
                "fieldName": "flightBookingNumber",
                "fieldType": "String"
            },
            {
                "fieldName": "flightCompanyName",
                "fieldType": "String"
            },
            {
                "fieldName": "flightPrice",
                "fieldType": "BigDecimal"
            }
        ],
        "bpmnProcessDefinitionId": "TravelPlanProcess",
        "bpmnTaskDefinitionId": "TaskFlight",
        "processEntityType": "TravelPlanProcess",
        "domainEntityType": "TravelPlanDomain"
    }

The `fields` list describes the fields that will be viewed or updated in the task.
Note that in this section you can only use fields that were declared in the domain entity. You cannot describe new fields here. The property `fieldReadOnly` allows marking a field as read-only. In this case, the task will not update the field. In the example above, the fields **startDate** and **endDate** are marked as read-only. The fields **flightBookingNumber**, **flightCompany** and **flightPrice** will be updated in this task.

The `bpmnProcessDefinitionId` and `bpmnTaskDefinitionId` fields are, respectively, the process and task id defined in the bpmn file.

The `domainEntityType` field is the name of the domain entity (created in the step 3).

The `processEntityType` field is the name of the process entity (created in the step 4).

Execute the following command in the project root directory:    

    yo pa-jhipster:entity-task FlightTask --regenerate

Say 'yes' to the confirmation question as indicated in the image below.

<img width="694" alt="Screen Shot 2019-09-16 at 02 02 21" src="https://user-images.githubusercontent.com/4369840/64936871-112c8d00-d826-11e9-80c6-8f672e6ed8c2.png">

At this point, you are able to execute the first task of the Travel Plan process.

<img width="908" alt="Screen Shot 2019-09-16 at 02 05 15" src="https://user-images.githubusercontent.com/4369840/64937286-b6943080-d827-11e9-900f-a39d4640df88.png">

In the Travel Plan Domain page (accessed through the menu **Entities -> Travel Plan Domain**), it is possible to see the updated in the domain instance.

<img width="1152" alt="Screen Shot 2019-09-16 at 02 07 11" src="https://user-images.githubusercontent.com/4369840/64937297-bdbb3e80-d827-11e9-92e7-c64f2055b43c.png">

In the Task List page (accessed through the menu **Entities -> Tasks**), it is possible to see the second task of this process instance.

<img width="1160" alt="Screen Shot 2019-09-16 at 02 06 26" src="https://user-images.githubusercontent.com/4369840/64937305-c3b11f80-d827-11e9-93b9-fbb53b0fac0a.png">

### Step 5.2: Create the Reserve a Hotel Task

To generate the task **Reserve a hotel**, we created the file `.jhipster/HotelTask.json`:

    {
        "relationships": [],
        "fields": [
            {
                "fieldName": "startDate",
                "fieldType": "LocalDate",
                "fieldReadOnly": true
            },
            {
                "fieldName": "endDate",
                "fieldType": "LocalDate",
                "fieldReadOnly": true
            },
            {
                "fieldName": "hotelBookingNumber",
                "fieldType": "String"
            },
            {
                "fieldName": "hotelName",
                "fieldType": "String"
            },
            {
                "fieldName": "hotelPrice",
                "fieldType": "BigDecimal"
            }
        ],
        "bpmnProcessDefinitionId": "TravelPlanProcess",
        "bpmnTaskDefinitionId": "TaskHotel",
        "processEntityType": "TravelPlanProcess",
        "domainEntityType": "TravelPlanDomain"
    }

Execute the following command in the project root directory:    

    yo pa-jhipster:entity-task HotelTask --regenerate

Say 'yes' to the confirmation question.

At this point, you are able to execute the second task of the Travel Plan process.

<img width="905" alt="Screen Shot 2019-09-16 at 02 24 30" src="https://user-images.githubusercontent.com/4369840/64937801-be54d480-d829-11e9-9444-59154f400d5b.png">

In the Travel Plan Domain page (accessed through the menu **Entities -> Travel Plan Domain**), it is possible to see the updated in the domain instance.

<img width="1162" alt="Screen Shot 2019-09-16 at 02 25 42" src="https://user-images.githubusercontent.com/4369840/64937824-ca409680-d829-11e9-99e4-ab867c8f390c.png">

In the Task List page (accessed through the menu **Entities -> Tasks**), it is possible to see the third task of this process instance.

<img width="1159" alt="Screen Shot 2019-09-16 at 02 26 29" src="https://user-images.githubusercontent.com/4369840/64937843-d75d8580-d829-11e9-8592-7153d1024085.png">


### Step 5.3: Create the Reserve a Car Task

To generate the task **Reserve a car**, we created the file `.jhipster/CarTask.json`:

    {
        "relationships": [],
        "fields": [
            {
                "fieldName": "startDate",
                "fieldType": "LocalDate",
                "fieldReadOnly": true
            },
            {
                "fieldName": "endDate",
                "fieldType": "LocalDate",
                "fieldReadOnly": true
            },
            {
                "fieldName": "carBookingNumber",
                "fieldType": "String"
            },
            {
                "fieldName": "carCompanyName",
                "fieldType": "String"
            },
            {
                "fieldName": "carPrice",
                "fieldType": "BigDecimal"
            }
        ],
        "bpmnProcessDefinitionId": "TravelPlanProcess",
        "bpmnTaskDefinitionId": "TaskCar",
        "processEntityType": "TravelPlanProcess",
        "domainEntityType": "TravelPlanDomain"
    }

Execute the following command in the project root directory:    

    yo pa-jhipster:entity-task CarTask --regenerate

Say 'yes' to the confirmation question.

At this point, you are able to execute the third task of the Travel Plan process.

<img width="910" alt="Screen Shot 2019-09-16 at 02 27 00" src="https://user-images.githubusercontent.com/4369840/64937850-e2b0b100-d829-11e9-9c5a-80cac937fa6c.png">

In the Travel Plan Domain page (accessed through the menu **Entities -> Travel Plan Domain**), it is possible to see the updated in the domain instance.

<img width="1158" alt="Screen Shot 2019-09-16 at 02 27 43" src="https://user-images.githubusercontent.com/4369840/64937861-eba18280-d829-11e9-97f6-a4dff0ca9659.png">

In the Task List page (accessed through the menu **Entities -> Tasks**), it is possible to see that there is no task for this process instance.

<img width="1158" alt="Screen Shot 2019-09-16 at 02 28 24" src="https://user-images.githubusercontent.com/4369840/64937872-f65c1780-d829-11e9-9194-8183e9a0113e.png">

**That is it. Congratulation. You just completed the blueprint example.**







