---
title: Customization
permalink: /docs/customization/
layout: docs
updated: 2019-03-02
---


# Bughunting


## Custom Bug Notification Mail Templates
default notification mail looks like this:

```
Dear domain owner of {{ vulnerability.domain.domain }},


we are currently running an instance of CySec-Framework (https://cysec-framework.gitlab.io), which allows security researchers
to publish found vulnerabilities and help admins to fix them.

{{ vulnerability.user.username }} is one of our bug hunters. He found a vulnerability in your application.

You can find more details here: {{ link }}

This link is valid until the bug is disclosed on {{ vulnerability.disclose_deadline|date:"d.m.Y" }}

```

you can use all attributes of the vulnerability and the generated detail link.

Create a new template called `bug_notification_mail.html` in directory `templates/bughunting`

For customizing the mail subject by creating a file `bug_notification_mail_subject.html` in directory `templates/bughunting`. You can use  all vulnerability attributes here.


## Custom Bot Messages
Todo

# Pentesting
## Custom Report Templates
TODO