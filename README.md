![MirrorGate](./docs/assets/logo.png)

MirrorGate is a WallBoard application meant to give teams fast feedback in all the different areas related to software development. This repo contains a sample docker-compose composition including the [main application](http://www.github.com/bbva/mirrorgate), an embeded mongoDB, a Jenkins and a Jira collector.

# How to make it work

## Prerequisites

You neet to have installed both:
- [docker](https://www.docker.com/)
- [docker-compose](https://docs.docker.com/compose/)

## How to execute it

First of all clone this repo.

```sh
git clone git@github.com:BBVA/mirrorgate-sample-deployment.git
```

## Running only mirrorgate

Execute `docker-compose up app` and open [http://localhost:8080/mirrorgate/backoffice/index.html](http://localhost:8080/mirrorgate/backoffice/index.html) in a browser. It should open the applicaton but you will need to execute some collectors to feed it with data. A good way to start is running the whole assembly instead.

## Running the whole assembly

Create a `.env` file in its root with the following contents changing the values according to your jira instalation:

```

JIRA_URL=https://my.jira.corp/jira
JIRA_USERNAME=admin
JIRA_PASSWORD=adminPassword

# Get the list of custom fields by calling https://[your-jira-domain-name]/rest/api/2/issuetype/
JIRA_FIELDS_STORYPOINTS=customfield_10002
JIRA_FIELDS_SPRINT=customfield_10008
JIRA_FIELDS_KEYWORDLIST=customfield_10245,customfield_10271

```

Then execute `docker-compose up` or `docker-compose up jenkins` if you want to skip the Jira collector and just want to check Jenkins. It will start and attempt to get the last 1 month information from your Jira. Note that you need a Jira instalation with the Agile plugin installed and some active sprints on it.

It will also execute a Jenkins instance in [http://localhost:9000](http://localhost:9000). Open it and follow the instalation instructions.

Once you have completed the Jenkins instalation, install the last version of the [Jenkins collector](https://github.com/BBVA/mirrorgate-jenkins-builds-collector) in it and configure the plugin to point to `http://app:8080/mirrorgate`.

Then create a Job and execute it.

Now open the [MirrorGate backoffice](http://localhost:8080/mirrorgate/backoffice/index.html) and create a dashboard with the value `.*` in the repositories field and the name of a Jira team inside the boards field. Navigate to the dashboard and you should see it working (if jira collector has already ended it's work which depends on the ammount of userstories you have)

