---
title: Master Slave Architecture
permalink: /docs/master-slave-architecture/
layout: docs
updated: 2019-01-22
---

# Master-Slave Application (in development)
this module allows you to seperate your OPAS-F clients to multiple hosts or containers. this clients will request a command queue, execute them and push the results to your server.

**At the moment there is no protection against an attacker to insert own commands in the database which gets than executed by your client**


# Setup
- add 'ENABLE_MASTER_SLAVE_APPLICATION = True' to your 'local_settings.py'
- if need execute 'python manage.py migrate'
- if not already done, create a pentesting project
- create an application
- application needs to have 'client_credentials' or 'password' as 'grant_type'

# Usage
## Prepare your environment
For this example. we will use our python api from [here](https://codeberg.org/OPAS-F/opas-f-python-api). Simply follow the setup guide.
We assume, that you already setup a OPAS-F Server, with a user already created.

## Pentesting Master
For setting up a pentesting master we need to do the following:

1. Create a project (or use an existing one)
2. Setup a new application
3. Add command to queue, that are fetched from the slave

```python
from opasf_api import OPASFClient


client = OPASFClient(username="your_username", password="your_password")

project = client.create_project("Master-Slave Test Project")

slave1 = client.create_user_application("confidential", "password", "Slave 1")

client.create_queue_item(PROJECT_ID, "nikto -h example.com", str(datetime.now().date()))

```

This example script can be used to create a pentesting master. Simply edit the values to your needs.


## Pentesting Slave
TODO

until this one gets better documentation, you may look at the example code at [this issue](https://codeberg.org/OPAS-F/opas-f-docs/issues/3)