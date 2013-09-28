---
layout: post
title: "Scheduled Web Service Calls with Spring Boot and Spring Web Services"
description: "Use Scheduling with Spring in combination with the easy peasy Spring Boot with a scheduiled Web Service Call"
category: 
tags: []
---
{% include JB/setup %}

# About

The task is to *call a Web Service periodically* (using a time-based job scheduler) from a *maschine with just Java 7 installed*.
All relevant data (the SOAP-Request, the server URLs to call, the delay between calls) *should be easily configurable*.

To accomplish this, we need
* a *generic Web Service (Client) framework* which is able to send *arbitrary configured requests to arbitrary URLs*
* a *configurable time-based job scheduler*
* some environment which can use both components *without installing anything*
* nice to have: the execution should be *as easy as possible (as well as the configuration)*

# Solution

As always, Spring to the rescue!
We have: *Spring Boot* and *Spring Web Services*. We use both with the brand new *STS 3.4.0*, using the new [Spring Guides] (http://spring.io/guides).

# Preparation

* To really take the easy path you have to have to install or update to [Spring Tool Suite 3.4.0] (https://spring.io/tools/sts/all) to be able to use the new Spring Guides.
* Now, you can use the [Working a Getting Started guide with STS] (http://spring.io/guides/gs/sts/) to prepare the Maven project. Choose the "Scheduling Task" Guide.

<img src="/assets/2013-09-27-scheduled-web-service-calls-with-spring-boot--spring-web-services/img/sts-gs.jpg" />

# Finish

..and we're done :)

Ok, let's go through our requirements
* a generic Web Service (Client) framework: We introduced the *WebServiceClient, which uses Spring's WebServiceTemplate*.
* a configurable time-based job scheduler: We use the *Spring's @Schedule annotation*, which can even be configured with a external `application.properties` outside the generated (uber-)jar.
* some environment which can use both components without installing anything: We use *Spring Boot* for this, which *facilitates easy creation of (uber-)jars* which can be startet just like `java -jar`
* nice to have: the execution should be as easy as possible (as well as the configuration): Execution is as easy as `java -jar gs-scheduling-tasks-0.1.0.jar`. The configuration is in the execution path (e.g. next to the jar) named `application.properties`.

# Details

So, how are the features activated? Mostly by convention-over-configuration.

## Structure/Layout

Our directory structure (done with "tree" in Cygwin)

<pre>
+-- pom.xml
+-- src
¦   +-- main
¦       +-- java
¦       ¦   +-- hello
¦       ¦       +-- Application.java
¦       ¦       +-- ScheduledTasks.java
¦       ¦       +-- WebServiceClient.java
¦       +-- resources
¦           +-- logback.xml
+-- application.properties        (copy file to target directory manually)
+-- input.xml                     (copy file to target directory manually)
+-- target
¦   +-- gs-scheduling-tasks-0.1.0.jar (start in this directory with java -jar ...)
¦   +-- application.properties
¦   +-- input.xml                 
</pre>

no surprises here. Even though it is surely possible with customizing Maven, I decided to manually copy the two configuration files to the target directory, so I can execute it with {{java -jar gs-scheduling-tasks-0.1.0.jar}} from the target directory immediately.

## Configuration

The application configuration, which must be named `application.properties` and must be on the execution path (where `java -jar` is called), ie. outside the generated uber-jar. So, it can just de deployed together with the generated jar.

<pre>
name=http://localhost:8080/ws/personService,http://localhost:8080/ws/personService
file=input.xml
delay=5000
</pre>

## Main class

We are using an Application main class, which mostly just configures Spring's component scan and enables auto configuration (coming from Spring Boot).

<pre>
@Configuration
@EnableAutoConfiguration
@ComponentScan
public class Application {

  public static void main(String[] args) throws Exception {
    SpringApplication.run(Application.class);
  }
  
}
</pre>

Now, the component scan will detect the `ScheduledTasks` component, which has the `callWebservice` method annotated with `@Scheduled`, so Spring will start this method immediately, *automatically configured with the delay from the application.properties file*.
All other configuration is also taken from the external `application.properties`.

<pre>
@Component
@EnableScheduling
public class ScheduledTasks {

  private static final SimpleDateFormat dateFormat 
                       = new SimpleDateFormat("dd.MM.yyyy HH:mm:ss");

  @Value("${name}") private String configServers;
  @Value("${file}") private String inputFile;

  private String[] servers;

  @Scheduled(fixedDelayString = "${delay}")
  public void callWebservice() {
    if (servers == null) {
      servers = configServers.split(",");
    }
    System.out.println("\n### [Web Services] on " + dateFormat.format(new Date()));
    long time = System.currentTimeMillis();
    for (String url : servers) {
      System.out.print("==> " + url);
      // read the sample input XML file configured as "file" in application.properties. 
      // Assume it is on the execution path (user.dir).
      WebServiceClient.create(
            url, 
            new File(System.getProperty("user.dir") + File.separatorChar + inputFile)
      ).customSendAndReceive();
      System.out.println(" ...ok [" + (System.currentTimeMillis() - time) + " ms]");
    }
  }
}
</pre>

## Web Service Client

The schedule method creates new WebServiceClients, which are configured with the URLs from the `application.properties` and the XML takes from the configured file.
This represents a generic Web Service Client, since we can call a Web Service at any location with any input XML (configurable!).

<pre>
public class WebServiceClient {

  private WebServiceTemplate webServiceTemplate;

  private File file;
  private String url;

  private WebServiceClient(String url, File file) {
    this.url = url;
    this.file = file;
    this.webServiceTemplate = new WebServiceTemplate();
  }

  public static WebServiceClient create(String url, File file) {
    return new WebServiceClient(url, file);
  }

  public void customSendAndReceive() {
    try {
      StreamSource source = new StreamSource(new FileReader(file));
      StreamResult result = new StreamResult(new StringWriter());
      org.springframework.ws.soap.saaj.SaajSoapMessageFactory sFactory = new org.springframework.ws.soap.saaj.SaajSoapMessageFactory();
      sFactory.setSoapVersion(org.springframework.ws.soap.SoapVersion.SOAP_12);
      sFactory.afterPropertiesSet();
      webServiceTemplate.setMessageFactory(sFactory);
      webServiceTemplate.afterPropertiesSet();
      webServiceTemplate.sendSourceAndReceiveToResult(url, source, result);
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    }
  }
}
</pre>

We are using this sample XML (must reside on the execution path where `java -jar` is called), it fits as client data for the [Spring Web Service server sample from springbyexample.org] (http://www.springbyexample.org/examples/setup.html#setup-project-setup-project-checkout)

    <bean:get-persons-request
      xmlns:bean="http://www.springbyexample.org/person/schema/beans">
      <name>?</name>
    </bean:get-persons-request>
