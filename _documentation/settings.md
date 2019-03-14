---
title: Settings
permalink: /docs/settings/
layout: docs
updated: 2019-03-02
---

If you start the server for the first time it should run out of the box using the default values for the different settings. You can configure the server by editing values from within the 'local_settings.py' file.

## Available Settings (Server)


| NAME | Default Value | Description |
| -------- | -------- | -------- |
| DEBUG | False | Enable Debug |
| ENABLE_REGISTRATION | False | Enable registration of new user accounts |
| PASSWORD_RESET_TIMEOUT_DAYS | 14 | Token used for password reset is valid for X days |
| SIGNUP_ACTIVATION_EMAIL_SUBJECT | "Activate your OPAS-F Account" | E-Mail subject of the account registration email |
| BUG_DISCLOSURE_DAYS | 30 | Days until discovered bugs gets disclosed |
| ENABLE_MASTER_SLAVE_APPLICATION | False | Enable Master-Slave application |
| ENABLE_BUGHUNTING_APPLICATION | False | Enable Bughunting application |
| ENABLE_BLOGGING_APPLICATION | True | Enable Blogging application |
| EMAIL_HOST | "localhost" | The host to use for sending email. |
| EMAIL_PORT | 25 | Port to use for the SMTP server defined in EMAIL_HOST. |
| EMAIL_HOST_USER | '' | Username to use for the SMTP server defined in EMAIL_HOST. If empty, Django won’t attempt authentication.|
| EMAIL_HOST_PASSWORD | '' | Password to use for the SMTP server defined in EMAIL_HOST.|
| EMAIL_USE_TLS | False | Whether to use a TLS (secure) connection when talking to the SMTP server. |
| EMAIL_USE_SSL | False | Whether to use an implicit TLS (secure) connection when talking to the SMTP server. |
| DEFAULT_FROM_EMAIL | 'webmaster@localhost' | Default email address to use for various automated correspondence from the site manager(s). |
| ENABLE_ADMIN_INTERFACE | True | Enable admin interface |
| ADMIN_URL_PATH | "admin/" | Change admin url path to hide admin interface if needed |
| SELF_DOMAIN | "http://localhost:8000" | Domain which should be used by bug disclosure script |
| BUG_DISCLOSURE_BOTS | [{'name': 'matrix', 'enabled':False, 'module':'bughunting.bots.matrix', 'args':[]}] | List of dictionaries with settings for bot modules |
| SIGNUP_CAPTCHA | True | Enable to solve captcha on signup |
| REQUIRE_SIGNUP_EMAIL | True | Accounts only become active after email confirmation |
| BUGHUNTING_NEED_MANUAL_APPROVING | False | Bugs gets only published after an OPASF admin has approved |

*for a full list see [django docs](https://docs.djangoproject.com/en/dev/ref/settings/)*
# Misc
## Anonymous Mailing (Server)
*requires tor and pysocks to be installed*

add the following to your local_settings.py file

```python
import socks
import smtplib
socks.setdefaultproxy(socks.SOCKS5, 'localhost', 9050)
socks.wrapmodule(smtplib)
```

## Postgres Database (Server)
*requires psycopg2-binary to be installed*

add the following to your local_settings.py file:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'opasf_db',
        'USER': 'opasf_user',
        'PASSWORD': 'somesecurepassword',
        'HOST': 'localhost',
        'PORT': '',
    }
}

```

*note: for the docker-compose setup using postgres, you need to set 'HOST' to 'db'*