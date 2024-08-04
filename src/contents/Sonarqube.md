---
author: kuldeep
datetime: 2024-08-04
title: Sonarqube
slug: sonarqube
featured: false
draft: false
tags:
  - sonarqube
ogImage: ""
description: SonarQube is a self-managed, automatic code review tool that systematically helps you deliver Clean Code.
---

# Sonarqube
SonarQube is a self-managed, automatic code review tool that systematically helps you deliver Clean Code. As a core element of our Sonar solution, SonarQube integrates into your existing workflow and detects issues in your code to help you perform continuous code inspections of your projects. The product analyses 30+ different programming languages and integrates into your Continuous Integration (CI) pipeline of DevOps platforms to ensure that your code meets high-quality standards.

## Sonarqube using docker
```bash
sudo docker run -p 9000:9000 -d --rm --name sonar sonarqube
```

## Sonarqube with report plugin
Fall this plugin guide 
https://github.com/cnescatlab/sonar-cnes-report/releases

create an docker image and use that instead

## Jenkins pipeline
```bash
pipeline {
    agent any
    tools {
        jdk "jdk17" // set this also in jenkins
        maven "maven3" // set this also in jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
        GIT_REPO_URL = 'https://github.com/sonukuldeep/sonarqube-maven-example-updated.git'
        GIT_BRANCH = 'master'
    }

    stages {
        stage('cleanup') {
            steps {
                cleanWs()
            }
        }
        stage('git checkout') {
            steps {
                git branch: "${GIT_BRANCH}", changelog: false, credentialsId: 'github-token', poll: false, url: "${GIT_REPO_URL}"
            }
        }
        
        stage('sonar analysis') {
            steps {
                sh """
                    mvn clean package
                    mvn sonar:sonar \
                    -Dsonar.host.url=http://127.0.0.1:9000 \
                    -Dsonar.login=$SONAR_TOKEN
                """
            }
        }
    }
}
```

## Pom.xml
```bash
To integrate a Java project with SonarQube, you can extract and adapt certain sections from the `pom.xml` file to other Java projects. Here are the key sections you'll want to copy and customize:

### 1. **Dependencies Section**
Include the SonarQube Maven plugin dependency in the `build` section of the `pom.xml`.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.sonarsource.scanner.maven</groupId>
            <artifactId>sonar-maven-plugin</artifactId>
            <version>3.6.0.1398</version>
        </plugin>
    </plugins>
</build>
```

### 2. **Properties Section**
You can define default SonarQube properties in the `properties` section of `pom.xml`. These properties include the SonarQube project name and key.

```xml
<properties>
    <sonar.projectName>YourProjectName</sonar.projectName>
    <sonar.projectKey>YourProjectKey</sonar.projectKey>
</properties>
```

### 3. **Build Plugin Management Section**
Configure the SonarQube plugin as part of the `pluginManagement` in the `build` section. This ensures that the correct version of the plugin is used.

```xml
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.sonarsource.scanner.maven</groupId>
                <artifactId>sonar-maven-plugin</artifactId>
                <version>3.6.0.1398</version>
            </plugin>
        </plugins>
    </pluginManagement>
</build>
```

### 4. **Profiles Section**
Add a profile for SonarQube analysis. This profile can be activated by default or through Maven command-line options. 

```xml
<profiles>
    <profile>
        <id>sonar</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <!-- Optional URL to server. Default value is http://localhost:9000 -->
            <sonar.host.url>http://localhost:9000</sonar.host.url>
        </properties>
    </profile>
</profiles>
```

### Full Example for Integration

Here’s a compact version of `pom.xml` that integrates with SonarQube:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>example-project</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>Example Project</name>

    <!-- Add to project -->
    <properties>
        <sonar.projectName>ExampleProject</sonar.projectName>
        <sonar.projectKey>example-project-key</sonar.projectKey>
    </properties>

    <dependencies>
        <!-- Add your project dependencies here -->
    </dependencies>

    <!-- Add to project -->
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.sonarsource.scanner.maven</groupId>
                    <artifactId>sonar-maven-plugin</artifactId>
                    <version>3.6.0.1398</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>

    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
    </profiles>

</project>
```

### Customization

- **`sonar.projectName`**: Replace with your project's name.
- **`sonar.projectKey`**: Replace with a unique key for your project.

You can copy these sections into the `pom.xml` file of other Java projects to enable SonarQube integration. Adjust the project-specific details as needed.
```
