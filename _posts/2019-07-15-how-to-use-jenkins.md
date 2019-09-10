---
layout: post
comments: true
title: How To Use Jenkins?
description: In this tutorial, I will explain what is Jenkins and use a gradle project in Jenkins as an example.
ids: 7
image: /assets/images/jenkinslogo.png
category: DevOps
---

<p class="message"> 
Jenkins is an open source automation software that helps in continuous integration and continuous delivery.
</p>

## What is Jenkins?
---

Jenkins is an open source server that will automate alot of tasks like building, testing and deploying software. Teams that do not use Jenkins have to wait for Person A in the team to commit his code, so Person B can take the update from the repository and manually build the project to verify the build. 

Jenkins using CI will eliminate the above step, thus making it faster to produce production software.

### What is Continuous Integration (CI)?

Continuous Integration is a practice in which developers commit their code daily to the repository. Every commit is then automatically built which will make it easier to detect different errors that may occur from the commits.

### What is Continuous Delivery (CD)?

Continuous Delivery is when the software is automatically built, tested and ready for production. Therefore, continuous delivery helps in automating the release process, the only manual thing that has to be done is deciding when the release should occur.

An image illustrating both CI and CD:

<img src="/assets/images/cicd.png" alt="graph of CI and CD">
<cite>Image from [Atlassian](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)</cite>

### Installing And Configuring Jenkins
---

First you need to download Jenkins, to be able to download it navigate to the page [Jenkins Download](https://jenkins.io/download/), and then choose on which platform you want to download it.

After downloading it, it will run on http://localhost:8080/, and you will get the following screen:

<img src="/assets/images/jenkinslock.png" alt="Jenkins first screen">

Then navigate to the link and retrieve the password to be able to unlock Jenkins. Aftering unlocking Jenkins, you need to click on *Install suggested Plugins* and Jenkins will automatically install plugins like Ant, Gradle, Git, Subversion, etc...

Then you need to create an admin user by filling the following fields:

<img src="/assets/images/adminjenkins.png" alt="Jenkins admin creation">

After adding information for admin user, you will reach the home page of Jenkins:

<img src="/assets/images/jenkinshomepage.png" alt="home page jenkins">

Now you have a Jenkins server running but with no project, in the next section we will add a gradle project to Jenkins.

## Adding a Gradle Project
----

First, you need to install the git plugin and the gradle plugin to be able to use your repository and the gradle build scripts. To install them click on *Manage Jenkins* and then click on *Global Tool Configuration* and add the plugins. But, since you already used *Install suggested Plugins* when setting up Jenkins then those plugins will already be installed.

Then click on *Please create a new jobs to get started*, enter a name for the project, choose freestyle project and press *OK*.

<img src="/assets/images/jenkinsgradle.png" alt="new item">

After that, navigate to the *Source Code Management* and click on *Git* to add your repository to Jenkins.

<img src="/assets/images/gitJenkins.png" alt="adding gradle project">

## Building the Project
---

To be able to build the project in gradle, we need to configure the build steps. First click on the project then click on *Configure* and then navigate to the *Build* tab. After clicking on *Add build step*, choose the *invoke gradle script* option since this is a gradle option.

Then choose the radio button *Gradle wrapper* so we can use the gradle version used in this project, and inside the task field write *build* so we can invoke the *build* gradle task.

<img src="/assets/images/gradlebuild.png" alt="gradle build steps">

Now we can build the project by clicking on the **Build Now** button on the left side menu. We can then check the console output to see the logs:

<img src="/assets/images/consoleoutput.png" alt="console output">

## Build Trigger
---

Build trigger will let you start a job that can be automatically done once a specified event occurs, example scheduling a job that gets triggers when there are changes in the repository.

<img src="/assets/images/buildtrigger.png" alt="build trigger">

So, to be able to build according to changes in the repository you can use the *Poll SCM* option that will check the repository for changes and automatically build a new version. For example we can specify that we can Jenkins to check for changes in the repository every 15 minutes by adding the following to the *schedule* field:

```
H/15 * * * *
```

You can check the syntax of the schedule field by clicking on the *help* icon:

This field follows the syntax of cron (with minor differences). Specifically, each line consists of 5 fields separated by TAB or whitespace:
- MINUTE HOUR DOM MONTH DOW
- MINUTE	Minutes within the hour (0–59)
- HOUR	    The hour of the day (0–23)
- DOM	    The day of the month (1–31)
- MONTH	    The month (1–12)
DOW	The day of the week (0–7) where 0 and 7 are Sunday.

*I hope you enjoyed reading this jenkins tutorial, please feel free to leave any comments or feedback on this post!*