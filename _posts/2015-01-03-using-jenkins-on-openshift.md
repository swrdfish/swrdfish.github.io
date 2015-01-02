---
layout: post
title: using jenkins on openshift
categories: [web-development, openshift]
tags: [openshift, jenkins, automation]
published: True

---

So I have got my django app running on Openshift. ([how to run django app on Openshift]({% post_url 2014-12-29-djando-on-openshift.md %}))
Now I wanted to automate the deployment of the app from a Github repo. The tool people use to do this is called Jenkins. So here's the rundown of what I have been able to achieve. 


First of all I havent been able to automatically build the app for deployment. What I've been able to do is setup a jenkins server, create a new job which receives an event from Github notifying a modification, after receiving which it simply pulls the changes from Github and pushes it to the Openshift server running the django app. On receiving the push the django app is rebuilt and deployed. Therefore I don't have to push the app to both Github and django. But it would be better to have continous integration as the Openshift server stops the django app during rebuild so it becomes unreachable. So next step would be to implement CI.


#Steps to get Jenkins up and running

###1. Create a jenkins app
{% highlight bash %}
    rhc app create jenkins jenkins-1
{% endhighlight %}

###2. Login to the jenkins using ssh

###3. Create a file `git-ssh.sh` in the data directory
Create a file named `git-ssh.sh` in the `$OPENSHIFT_DATA_DIR` and put the following code inside it.

{% highlight bash %}
    #!/bin/bash
 
    ID_RSA="${OPENSHIFT_DATA_DIR}id_rsa"
    KNOWN_HOSTS="${OPENSHIFT_DATA_DIR}known_hosts"
 
    ssh -o UserKnownHostsFile=$KNOWN_HOSTS -i $ID_RSA $1 $2 
{% endhighlight %}

###4. Generate ssh keys
We need to generate the ssh keys needed for git pull/push to work.
{% highlight bash %}
    ssh-keygen -t rsa
{% endhighlight %}
Enter the path to the id_rsa file pointed to by git-ssh.sh ie `${OPENSHIFT_DATA_DIR}id_rsa`
Keep the passphrase field blank.

###5. Put the public key on Github and Openshift


