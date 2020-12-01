
# Project Progress - Deployed Webapp w/ Jenkins & Tomcat

## Introduction

FInally! We make some cool progress that can be displayed in the browser.

I've set up an apache tomcat server, created the appropraite tomcat roles and users such that I can now use tomcat deployer role from within jenkins to: 
- get changes from the webapp's github repo every minute (talk about continuous integration, am I right?)
- deploy those changes to a VM

## Prerequisite

Basic understanding of linux, jenkins, apache tomcat, and aws ec2. 

## Use Case

Speed to market is key, and this ci/cd pipeline gets developer changes every minute and deploys them to the vm! Very cool. 

## Cloud Research

I created my servers seperately so that troubleshooting would be more modular and less intensive than creating a single instance that hosted everything. However, if you were to combine both your jenkins and tomcat into a single server you would need to change the default ports one of them uses since, by default, they both use port 8080.

To use tomcat I needed to alter some of the config files such that the server could be accessed from the browser and not just from localhost. 

I also altered tomcats config files so that I now had a user with appropraite role permissions to deploy our automated jobs.

I set up a jenkins job that uses the tomcat user credentials to get changes from the repo and then deploy the changes to the webapp.

As a test, I then altered the webapp contents so that anyone accessing the page would see some contact links as opposed to the original generic line "Welcome to my devops project!", saved changes and refreshed the browser.

The automated tomcat job uses cron to scan the repo every minute, and then deploy changes, and it worked! My broswer displayed my updated webapp!

## ☁️ Cloud Outcome

We have a functioning (although pretty basic) CI/CD pipeline! Nice! I intend to add more complexity everyday, with tools like docker, kubernetes, and ansible. 

## Next Steps

- integrate docker into the project
- study for aws ccp
- send out my resume
- begin cka prep again
- ????
- profit

## Social Proof

[Tweet](https://twitter.com/lrnallday/status/1333692330893864961)
