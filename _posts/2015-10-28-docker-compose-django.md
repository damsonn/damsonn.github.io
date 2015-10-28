---
layout: post
title:  "Docker compose and Django"
date:   2015-10-28 10:15:20
categories: docker
---

I think docker compose is a great tool. It is a very portable and efficient way to run a modern web application. *Docker* provides [an example for Django](https://docs.docker.com/compose/django/) (and rails), but it is more of a development sandbox. I wanted to create a production ready, standard *Django* app using *docker compose*.

It uses the following stack:

- Django running on gunicorn
- Celery worker
- Postgres database
- RabbitMQ
- Nginx frontend server

### TL;DR ###
I created a sample project if you want [the full example](https://github.com/damsonn/django-docker-compose).

### Consideration ###
*docker compose* is not deemed production ready. There are some limitations, mainly related to scalability. But I think it is fine for small applications. It is obviously much better to understand how docker compose works.
more info on [docker's website](https://docs.docker.com/compose/production/).

### Tutum ###
The big news is [tutum](https://www.tutum.co/) has been [acquired by Docker](http://blog.docker.com/2015/10/docker-acquires-tutum/). Tutum is a could service to manage and deploy Docker applications. It uses a **stack** descriptor, which is compatible with docker-compose yaml format. So you can use my example project to deploy and mange your app with tutum.

### Dockerfile ###
{% highlight bash %}
FROM python:3.5

ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code

RUN mkdir /code/requirements
ADD requirements.txt /code/
ADD requirements/* /code/requirements/
RUN pip install -r requirements.txt

ADD . /code/

RUN mv proj/settings/docker.py proj/settings/local.py

RUN SECRET_KEY=temp_value python manage.py collectstatic -v 0 --clear --noinput
{% endhighlight %}

### docker-compose.yml ###
{% highlight yaml %}
db:
  image: postgres
  env_file: secrets.env
rabbitmq:
  image: rabbitmq
  env_file: secrets.env
worker: &app_base
  build: .
  restart: always
  command: celery -A proj worker -l info
  links:
    - db
    - rabbitmq
  env_file: secrets.env
  environment:
    - C_FORCE_ROOT=true
app:
  <<: *app_base
  command: gunicorn -w 3 -b 0.0.0.0 proj.wsgi
  volumes:
    - /code/static
  ports:
    - "8000:8000"
nginx:
  build: nginx
  ports:
    - "80:80"
  links:
    - app
  volumes_from:
    - app
{% endhighlight %}

### Source ###
Please go on github if you want to try that out [https://github.com/damsonn/django-docker-compose](https://github.com/damsonn/django-docker-compose)
