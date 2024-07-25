# Project Setup and Jenkins Pipeline

## Prerequisites

Before setting up the Jenkins pipeline, ensure you have the following dependencies installed on your system:

### Maven

- **Version:** 3.9.2 (or required version)
- **Installation:**
  - You can install Maven using the package manager or download it from the [official Apache Maven website](https://maven.apache.org/download.cgi).

### Java

- **Version:** OpenJDK 17
- **Installation:**
  - On Debian-based systems (like Ubuntu), install OpenJDK 17 using:
    ```bash
    sudo apt update
    sudo apt install openjdk-17-jdk
    ```

## Jenkins Configuration

### Install Jenkins

1. **Install Jenkins:**
   - Follow the [official Jenkins installation guide](https://www.jenkins.io/doc/book/installing/) for your operating system.

### Configure JDK in Jenkins

1. **Open Jenkins Configuration:**
   - Go to **Manage Jenkins** > **Global Tool Configuration**.

2. **Add JDK:**
   - Under **JDK**, click **Add JDK**.
   - **Name:** JDK 17 (or any name you prefer).
   - **JAVA_HOME:** `/usr/lib/jvm/java-17-openjdk-amd64`
   - **Install automatically:** Leave unchecked.

### Configure Maven in Jenkins

1. **Open Jenkins Configuration:**
   - Go to **Manage Jenkins** > **Global Tool Configuration**.

2. **Add Maven:**
   - Under **Maven**, click **Add Maven**.
   - **Name:** Maven-3.9.2 (or the name you prefer).
   - **MAVEN_HOME:** The installation path (if installing manually).

### Add Jenkins Pipeline

1. **Create or Update Your Jenkinsfile:**

   Ensure your Jenkinsfile (located in the repository) is configured correctly. Below is an example Jenkinsfile:

   ```groovy
   pipeline {
       agent any

       tools {
           jdk 'JDK 17'
           maven 'Maven-3.9.2'
       }

       environment {
           MAVEN_HOME = tool name: 'Maven-3.9.2', type: 'maven'
           JAVA_HOME = tool name: 'JDK 17', type: 'jdk'
       }

       stages {
           stage('Checkout') {
               steps {
                   git url: 'https://github.com/Nency-Ravaliya/Day13.git', branch: 'main', credentialsId: 'github-credentials-id'
               }
           }

           stage('Build') {
               steps {
                   script {
                       withEnv(["PATH+MAVEN=${MAVEN_HOME}/bin", "PATH+JAVA=${JAVA_HOME}/bin"]) {
                           sh 'mvn clean package'
                       }
                   }
               }
           }

           stage('Archive Artifacts') {
               steps {
                   archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
               }
           }
       }

       post {
           always {
               echo 'Pipeline finished.'
           }
           success {
               echo 'Pipeline succeeded.'
           }
           failure {
               echo 'Pipeline failed.'
           }
       }
   }
```

## Output
- ![image](https://github.com/user-attachments/assets/ba7b6d33-c5e4-4270-a772-5f3e85187334)
- ![image](https://github.com/user-attachments/assets/b475b3c0-e6a8-48e9-9407-3b3876f36a5b)
- ![image](https://github.com/user-attachments/assets/01220343-f7d5-4cd2-bffd-57e01ca3a724)
- ![image](https://github.com/user-attachments/assets/7e7d72cd-c733-45d0-a3b9-1b11a0d165e1)




