---
layout: post
comments: true
title: Blue Ocean In Jenkins
description: Blue Ocean was created to easily visualize the jenkins pipeline, and also so the developers gets a good experience after using Jenkins.
ids: 7
category: DevOps
---

<p class="message"> 
Blue Ocean for Jenkins was created to make it easier for teams to use Jenkins. When using blue ocean you can visualize the jobs getting executed and personalize it, so it can suit every member in a team. Blue Ocean contains many dependent plugins and all of those plugins will form the blue ocean suite of plugins.
</p>

## Download Blue Ocean

First, to get started in using blue ocean, we need to download it. We can do that by entering our jenkin instance and clicking on *manage jenkins* then *manage plugins* and searching for *blue ocean* in the available plugins.

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/blueocean.jpg" src="/assets/images/blueocean.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/blueocean.jpg 300w,
    /assets/images/blueocean.jpg 600w,
    /assets/images/blueocean.jpg 900w">

Then click the checkbox and download the blue ocean plugin and restart Jenkins to be able to use blue ocean. After restarting Jenkins, a new button called "open blue ocean" will be displayed.

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/openblueocean.jpg" src="/assets/images/openblueocean.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/openblueocean.jpg 300w,
    /assets/images/openblueocean.jpg 600w,
    /assets/images/openblueocean.jpg 900w">

When clicking on the button, you will navigate to a new screen with more modern user interface. One of the goals of blue ocean is to completely replace the old Jenkins UI. This is the dashboard in blue ocean:

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/blueoceandashboard.jpg" src="/assets/images/blueoceandashboard.jpg" alt="blue ocean dashboard" data-srcset="/assets/images/openblueocean.jpg 300w,
    /assets/images/blueoceandashboard.jpg 600w,
    /assets/images/blueoceandashboard.jpg 900w">

As, you can see it is completely different than the old Jenkins UI. At the dashboard you can see all your projects, example the pipeline and freestyle project. You can also go back to the old jenkins UI by clicking on the *exit* button next to the *login* button.

After logging in, you will be able to create new pipeline projects and a new button will be displayed that will take you to *manage jenkins* section in the old Jenkins UI.

## Dashboard In Blue Ocean

In the dashboard, you can see all the projects inside the Jenkins instance. If you click on any project name, you will then be navigated to another screen that will show you details about that project. You can also click on *Run* to execute the Jenkins pipeline project. After clicking *run* you will see the following screen:

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/jenkinsbuild.jpg" src="/assets/images/jenkinsbuild.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/openblueocean.jpg 300w,
    /assets/images/jenkinsbuild.jpg 600w,
    /assets/images/jenkinsbuild.jpg 900w">

As you can see, this is a new moder UI being used in Jenkins. In the black background, you can see the output of the build and on the top right of the output you can also click on *download* to download the console output.

At the top navigation bar, you can check all the changes done for this build and all the tested executed in the build.

You can also favorite each project by clicking on the *star* icon in the project details screen. After doing that, a favorite list will be created above the pipeline projects.

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/favorite.jpg" src="/assets/images/favorite.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/favorite.jpg 300w,
    /assets/images/favorite.jpg 600w,
    /assets/images/favorite.jpg 900w">

As you can see, each background color of each project in the favorite list, represents the status of the last build. In the image above, the first project was aborted while the second project had a sucessful build.

## New Project In Blue Ocean

Now let's try and create a new pipeline project using blue ocean, so first log in to blue ocean and click on *create new pipeline*. Then choose the location of your code and click on the radio button.


<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/codestore.jpg" src="/assets/images/codestore.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/codestore.jpg 300w,
    /assets/images/codestore.jpg 600w,
    /assets/images/codestore.jpg 900w">

In this example, I will choose *Github*. Then, navigate to Github and generate the access token so you can use it in Jenkins. It will load all the repositories in your profile, you can then choose one project and create pipeline for that project.

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/repositories.jpg" src="/assets/images/repositories.jpg" alt="blue ocean jenkins repositories" data-srcset="/assets/images/repositories.jpg 300w,
    /assets/images/repositories.jpg 600w,
    /assets/images/repositories.jpg 900w">

If there is no Jenkinsfile in the repository, then you will have to create the pipeline in pipeline editor in blue ocean. You can click on the plus sign to create a new *stage*, and then on the right side, you can name the *stage* and add different steps.

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/pipelinegraph.jpg" src="/assets/images/pipelinegraph.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/repositories.jpg 300w,
    /assets/images/pipelinegraph.jpg 600w,
    /assets/images/pipelinegraph.jpg 900w">

After adding all the required *stages*, you can click on *save* and a new Jenkisfile will ne crutated in the repository. After running the project, you will see the following output:

<img data-sizes="auto" class="lazy-loading" data-src="/assets/images/blueoceanout.jpg" src="/assets/images/blueoceanout.jpg" alt="blue ocean jenkins" data-srcset="/assets/images/blueoceanout.jpg 300w,
    /assets/images/blueoceanout.jpg 600w,
    /assets/images/blueoceanout.jpg 900w">

As you can see from the output that the *stage* was executed and the *steps* which consisted of display `hello world` has worked.

*I hope you enjoyed reading this blue ocean tutorial, please feel free to leave any comments or feedback on this post!*
