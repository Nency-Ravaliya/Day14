pipeline {
    agent any

    tools {
        jdk 'JDK 17' // Make sure this name matches what you configured
        maven 'Maven-3.9.2' // Ensure this matches the name configured for Maven
    }

    environment {
        MAVEN_HOME = tool name: 'Maven-3.9.2', type: 'maven'
        JAVA_HOME = tool name: 'JDK 17', type: 'jdk'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Nency-Ravaliya/Day13.git', branch: 'main', credentialsId: 'gtk0'
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

