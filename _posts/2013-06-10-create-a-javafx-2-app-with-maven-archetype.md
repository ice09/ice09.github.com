---
layout: post
title: "Create a JavaFX 2 App with Maven (Archetype)"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Short introduction, derived from the more detailed [Quick Start Maven Archetype] (https://github.com/zonski/javafx-maven-plugin/wiki)

1. Create from Maven Archetype: `mvn archetype:generate -DarchetypeGroupId=com.zenjava -DarchetypeArtifactId=javafx-basic-archetype`
2. Fix Classpath-Issue (JavaFX is not on the classpath), set JAVA_HOME: `set JAVA_HOME=c:\java\jdk1.7.0_21`
3. Fix Classpath-Issue, run Maven goal: `mvn com.zenjava:javafx-maven-plugin:1.5:fix-classpath`
4. Update Eclipse or STS preferences with modified JRE
<img src="/assets/2013-06-10-create-a-javafx-2-app-with-maven-archetype/img/eclipsestspref.png" />
5. [Create Windows Batch Bundle] (https://github.com/zonski/javafx-maven-plugin/wiki/Building-a-windows-bat-file-bundle): `mvn jfx:build-win-bat` or `mvn jfx:build-win-bat-bundle`