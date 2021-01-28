# Triggers
Documentation for triggers, running workflow executions

# Table of contents
* [Introduction](#introduction)
* [About](#about)
* [Webhook](#webhook)
* [Shuffle Subflow](#subflow)
* [Schedule](#schedule)
* [User Input](#user_input)
* [Email](#email)

## Introduction
Triggers are the operators used to execute a [workflow](/docs/workflow) automatically. They are connected to a actions within workflows - often the starting node. Triggers usually take an execution argument that will be used to execute the workflow in question.

This image shows a simple workflow with two triggers: Webhook & Scheduler. 
![logos-triggers](https://github.com/frikky/shuffle-docs/blob/master/assets/logos-triggers.png?raw=true)

## About
Triggers, along side apps and variables, can be found on the left hand side, in a tab called "Triggers". 

![Triggers-view-1](https://github.com/frikky/shuffle-docs/blob/master/assets/triggers-view-1.png?raw=true)

Triggers are developed for and by Shuffle specifically to give the user access to multiple ways to run a workflow. There are currently five ways to execute workflows:

* Cloud: Webhook, User Input, Schedule, Email, Rest API
* Onprem: Webhook, User Input, Schedule, Rest API

There are currently four triggers, with a fifth "hidden" one:
* Webhook 		- Handle realtime HTTP requests from anywhere. 
* Schedule 		- Runs your workflow on a schedule. Cronjob or seconds.
* User Input 	- Waits for user input before continuing. You can answer yes/no.
* Rest API 		- Essentially a trigger from either: 1. Another workflow. 2. Any third party software

Cloud only:
* Email 			- 23.05.2020: Can listen to office365 incoming mails

### Webhook
[Webhooks](https://en.wikipedia.org/wiki/Webhook) is the real-time handler for Shuffle data. Webhooks were initially implemented to handle data from [Office365 connectors](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/connectors/connectors-using) and [TheHive](https://github.com/TheHive-Project/TheHiveDocs/blob/master/admin/webhooks.md), but turned into a generic trigger as we saw the use-case for it. 

HTTP Method(s):
* POST

PS: Data in the POST request will be the execution argument.

#### Example
To start using webhooks, you should have a service that supports webhooks (e.g. the two above mentioned). If you just want to test, you can also use cron.

Once you have a service ready:
1. Drag the "Webhook" trigger from the left hand side into the main view, and you should see it automatically connect to your starting node. 
2. Click "start" to deploy this webhook (it might be auto-started). This will also be stopped if you remove the node or the workflow. A webhook can not be in multiple workflows, but you can deploy as many as you want.
3. Find your webhook URI in the "Webhook URI" window, and copy it to your service

![Triggers-view-2](https://github.com/frikky/shuffle-docs/blob/master/assets/triggers-view-2.png?raw=true)

Test it! Say your URI is: https://shuffler.io/functions/webhooks/webhook_336a7aa2-e785-47cc-85f4-31a4ab5b28b8 and you want to use {"test": "testing"} in your workflow:
```bash
curl -XPOST https://shuffler.io/functions/webhooks/webhook_336a7aa2-e785-47cc-85f4-31a4ab5b28b8 --data '{"test": "testing"}'
```

### Subflow 
**TBD**

### Schedule 
Schedules are based on [google's cloud scheduler](https://cloud.google.com/scheduler/) and are schedules that run based on [cron](https://en.wikipedia.org/wiki/Cron). It takes two arguments - the schedule you want to run it on and the Execution argument used in the workflow. A simple cron converter can be found [here](https://en.wikipedia.org/wiki/Cron). When you are ready to run, click "start". 

**On-prem**
Schedules on-prem are ran on the webserver itself. Rather than cron, it uses a scheduler that runs it every X seconds. This will become more sophisticated over time. Schedules persists in the database even when you turn off Shuffle, so don't be afraid to update.

#### Example
1. Drag a schedule into the view from the left side
2. Click the schedule
3. Type in the amount of seconds between execution (down to every 1 second)
4. Type the execution argument you want to execute with. This can be used by actions.
5. Click "Start"

![Triggers-view-3](https://github.com/frikky/shuffle-docs/blob/master/assets/triggers-view-3.png?raw=true)

### User input
The user input node is a way to temporarily pause an execution until someone MANUALLY clicks something. This can currently be through Email and SMS, but we will introduce many other options, including chat systems and running other workflows.

The point of the node is to wait for manual approval before running some automation. E.g. management approval for new scan schedule. E.g. analyst approval for cleanup up a host. 

![User input node Shuffle](https://github.com/frikky/shuffle-docs/blob/master/assets/triggers-view-4.png?raw=true)

* Email		- What emails do you want to send the workflow information to?
* SMS 		- What numbers do you want to send the workflow information to?

When the email/sms is sent, it will look like the image below. We will optimize this over time to reflect everything from Shuffle, but during our initial proof of concept, this is for you to understand. You will get to answer: yes or no? If you answer yes, the workflow execution will continue from where it left off. If you answer no, it will be stopped. This action can be sent to multiple people, but the link will only work once.
![User input email Shuffle](https://github.com/frikky/shuffle-docs/blob/master/assets/triggers-view-5.png?raw=true)

### Email
Cloud only for now. Can handle OAuth2 with Office365, but will get Google Workspaces as well. Will be accessible on-premises at some point.
