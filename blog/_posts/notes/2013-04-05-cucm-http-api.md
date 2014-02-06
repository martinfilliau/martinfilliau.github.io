---
layout: post
title: Building lightweight HTTP services in Java
category: Technology, Java
---

Our telecoms projects requires access to a SOAP Web Service called [Cisco "CUCM" Administrative XML](http://developer.cisco.com/web/axl/docs), used to manage Cisco IP phones. Our project being a Django (Python) application, we used the de facto SOAP library for Python called [Suds](https://fedorahosted.org/suds/).

Unfortunately, we encountered many problems as the CUCM service is composed of a lot of custom types, and we reached a point where suds was not good enough (except by starting to use a customised version of suds such as what is discussed [here](https://fedorahosted.org/suds/ticket/342)). Having difficulties to find an alternative in Python, we began to look at alternatives on different platforms.

Java comes to the rescue
------------------------

The Java platform is well-known for working with SOAP web services, and the tooling seemed to be appropriate and free to use. Generating an usable client to consume the SOAP web service is very easy (using [wsimport] (http://docs.oracle.com/javase/6/docs/technotes/tools/share/wsimport.html)) but the main question was: *how do we expose the web service in a better way?*

Enter [Dropwizard](http://dropwizard.codahale.com/), a Java framework to develop HTTP services, quite popular (originally developed by [Yammer](http://www.yammer.com)) and very easy to learn. It is very pragmatic and contains everything needed out-of-the box (configuration, logging, deployment) to build HTTP/JSON services.

After having spent days trying to access the web service in Python, we got an initial working version in a matter of hours. We now expose entities and methods doing exactly what we need, exposing just what is needed, and ready to be consumed by our front-end Django application.

The component is really easy to package and deploy, Dropwizard uses a [shaded ("fat") JAR](http://maven.apache.org/plugins/maven-shade-plugin/) containing [Jetty web server](http://www.eclipse.org/jetty/) which can be run directly.

A good tradeoff?
----------------

Although it doesn't seem ideal at first sight, introducing this middleware seems to be a good tradeoff, also allowing us to have a better separation of concerns and easily move part of the service on a different machine if need be. Having a separated component also means that we can open-source it to be potentially reused.

We are quite happy with the result as we have been able to develop quite rapidly an efficient service, you can see the result of our work on [GitHub](https://github.com/ox-it/cucm-http-api), this project is actively developed and may change in the near future.

Dropwizard appears to be a growing trend away from the J2EE days, the following article is an interesting read : [J2EE is dead](http://java.dzone.com/articles/j2ee-dead-long-live-javascript).

-------

Cross-posted from the [Mobile Oxford blog](http://blog.m.ox.ac.uk/posts/2013/04/05/cucm-http-api/)
