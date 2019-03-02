---
title: Setup
permalink: /docs/setup/
layout: docs
updated: 2019-03-02
---

# Setup Open Pentesting and Security Framework Server
First get the code from the [opas-f-server repo](https://codeberg.org/OPAS-F/opas-f-server)

# Docker
the simplest way to set up your Open-Pentesting-And-Security-Framework using docker. First step is to copy the 'local_settings.template.py' file to 'local_settings.py' and edit it to fit your needs.

`docker-compose up`


## Docker with Postgres database
TODO

# Without Docker (Linux)
## Database
- install needed software
    - `sudo apt install libpq-dev postgresql postgresql-contrib`
- setup database
    - `sudo su - postgres`
    - `psql`
    - `CREATE DATABASE opasf_db;`
    - `CREATE USER opasf_user WITH PASSWORD 'somesecurepassword';`
    - `GRANT ALL PRIVILEGES ON DATABASE opasf_db TO opasf_user;`
    - `\q`
    - `exit`
- update 'local_settings.py'

```
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
- optionally enable postgresql systemd service
    - `systemctl enable postgresql`


## Open-Pentesting-And-Security-Framework 
- install git, virtualenv (optional)
    - `sudo apt install git virtualenv`
- clone code
    - `git clone https://codeberg.org/OPAS-F/opas-f-server.git`
- cd into directory
    - `cd opas-f-server`
- enter virtual environment
    - `source bin/activate`
- install requirements
    - `pip install -r requirements.txt`
    - `pip install django[argon2]`
- prepare settings
    - `cp local_settings.template.py local_settings.py`
- optionally: edit settings to your need
- migrate database
    - `python manage.py migrate`
- if you want to enable admin interface
    - `python manage.py collectstatic`
- create super user
    - `python manage.py createsuperuser`
- start gunicorn
    - `gunicorn opas_f_server.wsgi`
- alternativly use systemd for gunicorn
    - paste the code below into '/etc/systemd/system/gunicorn.service'
    - `systemctl enable gunicorn`
    - `systemctl start gunicorn`


```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=cysec
Group=www-data
WorkingDirectory=/usr/local/src/opas-f-server
ExecStart=/usr/local/bin/gunicorn --timeout 180 --workers 4 --bind 127.0.0.1:8000 opas_f_server.wsgi:application --pythonpath /usr/local/src/opas-f-server

[Install]
WantedBy=multi-user.target

```

## Setup Bughunting auto disclosure
Todo: explain how to set up without docker

If you use docker, you do not need to configure anything here.

# Docker Hidden-Service
We created a simple script that allows you to easily create V3 - Hidden-Services that are running the api server.
If you did not already create a 'local_settings.py' file, you should create one by reading steps above.

For the hidden services to be created run `sh run_as_tor_hidden_service_v3.sh`. After they have been created and started, you need to change the 'ALLOWED_HOSTS' setting in the 'local_settings.py' file to your new hidden service addresses (they end with .onion). The script will output the generated addresses.


## Docker Hidden-Service with Postgres database
run `docker-compose -f docker-compose-tor-postgres.yml up`. Configure your local_settings.py and volume paths in 'docker-compose-tor-postgres.yml' to fit your needs.


## Next Steps

Now your server should be up and running, hopefully ;). If you need some help, feel free to ask for help at our matrix room. 

(TODO: add room identifier)

You are now using the server with our predefined settings. Some tips of how to change settings see the [settings page](settings)

Happy Hacking!
