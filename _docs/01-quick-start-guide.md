---
title: "Quick-Start Guide"
permalink: /docs/quick-start-guide/
excerpt: "How to quickly learn, install and setup the PA JHipster generator."
last_modified_at: 2019-12-03T21:36:11-04:00
redirect_from:
  - /theme-setup/
toc: true
---

The quickest way to install and use the PA Generator is following the steps below:

1. **Step 1: Configuring a docker container**
1. **Step 2: Creating an app**

## Step 1: Configuring a docker container

    docker container run --name pajhipster -v ~/pajhipster:/home/jhipster/app -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t untacit/pajhipster:0.0.1

This step is only executed once. After create the conteiner **pajhipster**, you can start and use it through a bash command line using the commands below.

```bash
docker container start pajhipster

docker container exec -it pajhipster bash
```

## Step 2: Creating an app

```bash
mkdir myapp

cd myapp

yo pajhipster

gulp inject

./mvnw
```

After this, you should be able to access the application through the address `http://IPDOCKER:8080`.

>To check your docker ip, execute the command ``docker-machine ip``.
