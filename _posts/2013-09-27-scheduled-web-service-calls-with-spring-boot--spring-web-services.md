---
layout: post
title: "Scheduled Web Service Calls with Spring Boot and Spring Web Services"
description: "Use Scheduling with Spring in combination with the easy peasy Spring Boot with a scheduiled Web Service Call"
category: 
tags: []
---
{% include JB/setup %}

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
¦   +-- gs-scheduling-tasks-0.1.0.jar
¦   +-- application.properties
¦   +-- input.xml                 
</pre>

<pre>
@Component
@EnableScheduling
public class ScheduledTasks {

	private static final SimpleDateFormat dateFormat = new SimpleDateFormat("dd.MM.yyyy HH:mm:ss");
	
	@Value("${name}") private String configServers;
	@Value("${file}") private String inputFile;
	
	private String[] servers;
	
    @Scheduled(fixedDelayString = "${delay}")
    public void reportCurrentTime() {
    	if (servers == null) {
    		servers = configServers.split(",");
    	}
        System.out.println("\n### [Web Services] on " + dateFormat.format(new Date()));
        long time = System.currentTimeMillis();
        for (String url : servers) {
        	System.out.print("==> " + url);
			WebServiceClient.create(url, new File(System.getProperty("user.dir") + File.separatorChar + inputFile)).customSendAndReceive();
        	System.out.println(" ...ok [" + (System.currentTimeMillis() - time )+ " ms]");
        }
    }
}
</pre>

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

<pre>
<bean:get-persons-request
	xmlns:bean="http://www.springbyexample.org/person/schema/beans">
	<name>?</name>
</bean:get-persons-request>
</pre>

<pre>
name=http://localhost:8080/ws/personService,http://localhost:8080/ws/personService
file=input.xml
delay=5000
</pre>


