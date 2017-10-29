---
layout: blog
title: Writing better Jenkinsfiles with GroovyConsole
date: 2017-10-28T19:57:20.837Z
thumbnail: /assets/img/uploads/jenkins-heart-of-ci-cd.png
rating: '1'
categories:
  - continuous integration
  - groovy
tags:
  - IT
  - Jenkins
  - Groovy
  - Chef
---
> managing Jenkins Jobs was a hard task

We all know Jenkins to automate build processes and do love it. The most annoying task about Jenkins jobs are their config files. It's quite hard to manage XML files with a configuration management like Chef, Puppet or Ansible.
Even automating job creation and management via Groovy scripts isn't easy, when you don't know all the available API.

> Pipelines changed my situation completely

With introducing Pipelines, Jenkins Version 2.0 changed my situation completely. It brings a new Jenkins job type called *Pipeline*.
The only details you need to create a new job, are: *Name*, *SCM URL* and *some credentials* for Jenkins to checkout the code of your project.
Jenkins reads `Jenkinsfile` within your repository root after checkout and configures all properties for your job.
With a job configuration within all code repositories, it's much easier to maintain all our Jenkins jobs.

Imagine you have to create a new Chef cookbook, you can use a personalized cookbook-generator-template with ChefDK, to create the cookbook with all files needed for Chef plus a `Jenkinsfile`.

Afterwards, you only have to push your newly created cookbook in a new Git repository and point your Jenkins to it.

> My challenge

During last days, I've spend much time in developing my `Jenkinsfile`-Template for our Chef cookbook continuous integration & deployment pipeline. (*I will not go into detail for now*)

I needed to write a function to determine the new version of the cookbook to bump it to, after successful passing all tests.

The function had to implement semantic versioning by reading all commit messages between last tagged version and the commit, which triggered the build.

```groovy
def get_auto_bump_version () {
  // TODO: place code here
}
```

While trying to write that function, I've stepped into many pitfalls because of not knowing enough about Groovy programming language.
I had used a test-project with Jenkins in try-and-error mode
(*or commit-and-push*) to find a working version.

> explain how to develop better `Jenkinsfile` within [groovyConsole](http://groovy-lang.org/groovyconsole.html)

Last Thursday evening, after some time of try-and-error-loops, I had the idea to use a GroovyIDE to write my function to get a faster feedback and to learn language pattern much faster.
The easiest and simplest IDE for Groovy seems to be [groovyConsole](http://groovy-lang.org/groovyconsole.html) which is part of Groovy package. With some additional function definitions, which are part of Jenkins Pipeline API like
* `sh` to execute shell commands or
* `readFile` to read contents of a file into a variable

I was able to implement a working function.
