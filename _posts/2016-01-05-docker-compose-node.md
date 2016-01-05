---
layout: post
title:  "Node.js x Docker compose"
date:   2016-01-05 12:25:00
categories: docker
---

A couple of months ago I did a post on [docker and django](http://damdev.me/docker/2015/10/28/docker-compose-django.html). Being a *Node.js* and *rails* developer primarily, I thought it would be a great idea to create one for **Node.js** too. The idea is still the same, to create a production ready, standard *Node.js/Express.js* app using *docker-compose*.

It uses the following stack:

- Node.js and Express.js
- MongoDB database
- nginx frontend server
- redis
- Kue worker

### TL;DR ###
I created a sample project if you want [the full example](https://github.com/damsonn/node-docker-compose).

### Consideration ###
*docker compose* is not deemed production ready. There are some limitations, mainly related to scalability. But I think it is fine for small applications. It is obviously much better to understand how docker compose works.
more info on [docker's website](https://docs.docker.com/compose/production/).

### Tutum ###
The big news is [tutum](https://www.tutum.co/) has been [acquired by Docker](http://blog.docker.com/2015/10/docker-acquires-tutum/). Tutum is a cloud service to manage and deploy Docker applications. It uses a **stack** descriptor, which is compatible with docker-compose yaml format. So you can use my example project to deploy and mange your app with tutum.

### Dockerfile ###
{% highlight bash %}
FROM node:5.3

# use changes to package.json to force Docker not to use the cache
# when we change our application's nodejs dependencies:
ADD package.json /tmp/package.json
RUN cd /tmp && npm install
RUN mkdir -p /opt/app && cp -a /tmp/node_modules /opt/app/

# From here we load our application's code in, therefore the previous docker
# "layer" thats been cached will be used if possible
WORKDIR /opt/app
ADD . /opt/app

ENV NODE_ENV production

EXPOSE 3000

{% endhighlight %}

### docker-compose.yml ###
{% highlight yaml %}
db:
  image: tutum/mongodb
  restart: always
  env_file: .env.docker

redis:
  image: redis
  restart: always

app:
  build: .
  volumes:
    - /opt/app/public
  command: node bin/www
  restart: always
  ports:
    - "3000:3000"
  links:
    - db
    - redis
  env_file: .env.docker
  environment:
    - DEBUG=node-docker-compose:server

worker:
  build: .
  command: node worker.js
  links:
    - db
    - redis
  env_file: .env.docker
  environment:
    - DEBUG=node-docker-compose:worker

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
Please go on github if you want to try that out [https://github.com/damsonn/node-docker-compose](https://github.com/damsonn/node-docker-compose)
