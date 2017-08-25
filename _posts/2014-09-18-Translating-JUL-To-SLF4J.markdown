---
layout: single 
title: "Translating legacy JUL to SLF4J"
date: 2014-09-18 16:32:33
categories: java
---

Recently, I've been working on new web service at work using JAX-WS (WTF?! soap:)).
During setup of logging I've noticed, that JAX-WS reference implementation uses
Java Util Logging library to log everything. This is understandable because it's distributed with
jdk, therefore it cannot (should not?) depend on any third party libraries, e.g. SLF4J.

Problem is, that we use Simple Logging Facade for Java with Logback backend and we
would like to have one setup for all logging stuff including libraries.
SLF4J provides different bridges for log4j, Commons Logging and so on, to solve this.
Most of them, you only have to add jar to classpath and everything works.
These bridges override implementation of classes for given logging backend, hovewer this isn't
possible with JUL, because java. classes cannot be overriden or excluded for that matter.

SLF4J resolves this problem by implementation of java.util.logging.Handler.
This handler intercepts messages in JUL and redirects them to SLF4J logger.
Handler has to be manually registered to JUL.
There are two options how to do this.
First, is to include new `logging.properties` file with one simple option.

{% highlight bash %}
handlers = org.slf4j.bridge.SLF4JBridgeHandler
{% endhighlight %}

This registers handler to root logger. This solution, hovewer, does not solve our
initial problem of having one configuration to them all.
As it requires to have two config files, one for JUL and one for logback (SLF4J).

The other (better, probably...) solution is to install handler during bootstraping of application.
As we implement web service quite native place, where to do this is in ServletContextListener.
Implementation of our listener might look something like this.

{% highlight java %}
package sk.iref.web.support;

public class SLF4JBridgeListener implements ServletContextListener {
  public void contextInitialized(ServletContextEvent sce) {
    if (!SLF4JBridgeHandler.isInstalled()) {
      SLF4JBridgeHandler.removeHandlersForRootLogger();
      SLF4JBridgeHandler.install();
    }
  }

  public void contextDestroyed(ServletContextEvent sce) {
    if (SLF4jBridgeHandler.isInstalled()) {
      SLF4JBridgeHandler.unistall();
    }
  }
}
{% endhighlight %}

Listeners checks if someone wasn't too clever and already registered handler.
Afterwards we remove all existing handlers from global logger. This step ensures that
default console handler was removed from JUL. Finally, we install handler.
On destroy, we simple uninstall handler from logging context.

To use listener, we have to registered it in standard web app descriptor (fancy name for web.xml).

{% highlight xml %}
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

  <listener>
    <display-name>JUL to SLF4J bridge bootstrap</display-name>
    <listener-class>sk.iref.web.support</listener-class>
  </listener>

  ...
</web-app>
{% endhighlight %}

If you're into annotations and you use Servlet 3.0,
you can add listener simply by annotating class with @WebServletContextListener.

As a sidenote, translation of messages in handler can be costly.
According to SLF4J docs around 60x folds (6000%).
To avoid performance issues, you should also add LevelChangePropagator as logging context listener in your logback configuration

And we're done. Hopefully this will help at least one person. :)

References
===

* [Logback](http://logback.qos.ch)
* [SLF4J](http://slf4j.org)
* [SLF4JBridgeHandler](http://www.slf4j.org/api/org/slf4j/bridge/SLF4JBridgeHandler.html)
* [ServletContextListener](http://docs.oracle.com/javaee/6/api/javax/servlet/ServletContextListener.html)
* [LevelChangePropagator](http://logback.qos.ch/manual/configuration.html#LevelChangePropagator)
