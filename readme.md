# MCM Monitoring usecase - Wealthcare UI responding slow

This document explains about how  SRE  is going to analyze, and resolve an Incident, using MCM monitoring. 

<img src="images/30-response-1.png">


## Usecase

The Web user interface of the `Wealthcare application is becoming very slow`.

An incident about, Wealthcare UI responding slow, is created in the Multi-cloud Management Monitoring.

Now SRE is going to analyze and resolve the incident, by using the events, generated from Golden Signals of the Application Runtime.

Here is the usecase and incident handling flow.

<img src="images/30-response-2.png">


## Note

This usecase is going to leverages the following objects for Application Monitoring and Incident management.
- Thresholds
- Synthetic Tests
- Runbooks
- Event Policies
- Incident Policies

How to create and configure them is discused in another git repo. 

https://github.com/GandhiCloudLab/mcm-monitoring-usecase-responsetime-configuration


## Abstract of the Incident and resolution steps

Here is the abstract of the Incident and resolution steps.

<img src="images/30-response-3.png">


Synthetic test creates an event.

Threshold config will also create an event.

Both the events are correlated and incident get created.

SRE look at the events in the incident.

He opens the Golden signals page of the web UI service.

With the help of transaction tracing, he figure out that, the issue is with financial plan service.

Then he opens the Golden signals page of the financial plan service.

He identify the problem is because of traffic increase. 

He opens the runbook, and get to know that, he has to increase the, replica of the POD.

He did so.

The incident is resolved. 


## Incident list

As a SRE, I  login into MCM Monitoring console, and look at my group, for incidents. 

There is an incident about, Wealthcare UI, responding slow

<img src="images/01-responsetime-1.png">

Two  events  are  associated with this incident.
It is in, Assigned state.
It is assigned, to wealthcare group.
Lets click on Investigate, to do the analysis.
It opens up the incident detail.

## Incident Detail

<img src="images/01-responsetime-2.png">

Two events are listed here.
<img src="images/01-responsetime-3.png">

Event details of Synthetic test
<img src="images/01-responsetime-4.png">

Event details about the wealthcare response time is high.
<img src="images/01-responsetime-4.png">

So this event is about the wealthcare response time is high.

Lets goto the resource dashboard screen to understand the event in detail.

## Golden Signals of Web UI

<img src="images/02-responsetime-1.png">

Here is the golden signals page, which shows Latency, Error, Traffic and Saturation. 

The Latency means the Response time, Error means, any error occurred during the execution of the service, Traffic is the request count, and Saturation is the memory usage. 

The latency and Error are the symptoms of the problem. 

The traffic and Saturation would be the cause of the problem.


As a SRE, lets see how to analyze the problem. 

We can observe that latency is showing higher value.

The traffic is also more. 

But there is no error and there is no problem with Saturation. 

Now we need to see, whether the problem is because of the dependent services. 

## Transaction tracing in Web UI
Click on the Transaction tracing icon of the API call.  

<img src="images/02-responsetime-2.png">

It goes to the tracing page. 

<img src="images/02-responsetime-3.png">

Choose any one of the transaction. 

<img src="images/02-responsetime-4.png">

It shows the tracing of the selected transaction.

You can see that UI service is calling financial plan service.

You can observe that delay is from the financial plan service. 

So lets goto the financial plan service by clicking it form service dependencies.

<img src="images/02-responsetime-5.png">

## Golden Signals of Financial plan

This page shows the golden signals of the financial plan service. 

<img src="images/03-responsetime-1.png">

Here also the saturation and errors are normal.

But the response time is high and traffic is also more.

It looks like the traffic is increased, financial plan service is finding difficult to respond.

Now I need to see, how do I resolve this problem. 


Let me check is there any Runbook associated with this incident. 

## Resolve using Runbook

I go to the incident page again, and look for the runbook.

<img src="images/04-responsetime-1.png">

Runbook, contains the sequence of steps to solve the problem. Either it can be manual or automated. So Let me choose the runbook associated to the event.

Assign the incident to me. 

<img src="images/04-responsetime-2.png">

The step 1, of the runbook requests, to go to search screen in MCM console.
<img src="images/04-responsetime-3.png">

The step 2, requests to enter search text as given.
<img src="images/04-responsetime-4.png">

The step 3, requests to choose the deployable object for financial page.
<img src="images/04-responsetime-5.png">

The step 4, requests to edit the yaml, and make the replica as 2.
<img src="images/04-responsetime-6.png">


After the pod count increase, the performance of the financial plan service improved, and user did not feel the slowness.

Now I resolve this incident.

<img src="images/04-responsetime-7.png">

<img src="images/04-responsetime-8.png">

<img src="images/04-responsetime-9.png">
