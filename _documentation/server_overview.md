---
title: Server Overview
permalink: /docs/server-overview/
layout: docs
updated: 2019-03-02
---

# Server of Open Pentesting and Security Framework
TODO

## API
The API is documented at 'host:port/api-docs'.

## Pentesting application
This application handles everything you need to manage your pentesting projects.

### Projects
You can add other users to your projects and specify a project role for them, e.g. "pentester". 

### Reports
OPAS-F allows you to easily create pentesting reports, based on an installed template. All reports can optionally be encrypted (server-side only for now :( ) using AES-GCM or ChaChaPoly. You can send the report link to your customer which allows him to download the report, without the need to create an account on the server. You may have a look at the [Customization page](Customization) to customize your report templates.



## Bughunting application
This application allows your users to submit bugs found in any application. OPAS-F will persist the bug details and send a notification mail to a contact address including an access link. This link allows the admin to access bug details, without the need to register to the server. If a bug gets not fixed since 30 Days all submitted details become public.
A bughunter could optionally increase the disclosure time, if an admin needs more time to fix complex bugs.

Also a bughunter can win virtuel awards for submitting bugs. 

This application is enabled by default, but needs some configuration to make email notifcation work. If you want to disable the application, have a look at [settings page](Settings).


### Bug Disclosed Bots
If a bug becomes disclosed, OPAS-F allows you to configure bots for social media, that posts a message to it. Currenlty only a matrix bot is available, but its quite easy to [code your own](Get-your-hands-dirty).



## Master-Slave application
Maybe you want to seperate your work using different hosts, and everyone should do a different job. This is why we implemented a simple master-slave architecture, which handles such situations. 

From your account, you need to create a new "application", e.g.: "Slave 1". You will receive tokens, which the slave needs to authenticate itself to the server. 

Further details can be found at the [master-slave wiki page](master-slave-architecture)

## Blogging application
TODO

## Accounts application
TODO