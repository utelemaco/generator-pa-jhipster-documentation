---
layout: collection
permalink: /process-awareness-web-app
title: "Process Awareness Web Application"
excerpt: "Process Awareness Web Application."
last_modified_at: 2019-12-03T15:46:43-04:00
toc: true
---

In a nutshell, *process awareness web application* is a term used to denote web applications that have their control flow controlled by a process engine. On the other hand, in *convencional* (or a "non-process awareness") web applications, the flow is hidden in source-code and sometimes hard to see.


## Convencional x Process Awareness Web Applications

The main difference between a convencional and a process awareness web application 

*Convencional*: The processes are hidden as mainly implicit. The flow is entirely controlled by the web app.

*Process Awareness*: The processes become explicit. The flow is controlled by a process engine.

## Example

Let's take as example a web application that controls a travel plan. To keep simple, we are assuming a travel plan is build in three steps:

* Buy the air tickets
* Book a hotel
* Rent a car

In a convencional web application, the programmer has to develop the pages that allow the final user to execute each actions and he also has to implement the flow control directly in the source-code. An eventual change in the flow requires an adjust in the web application source-code.

In a *process-awareness* application, the development of the pages is not directly tied to the flow control. Thus, the pages are developed and the process (that specifies the flow control) is deployed in the process engines integrated to the web application. Is the process engine that guide the final user in which they can do.



