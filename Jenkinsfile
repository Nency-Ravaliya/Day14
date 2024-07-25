pipeline {
    agent any
    
    environment {
        MAVEN_HOME = tool name: 'Maven-3.9.0'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Checking out branch: ${env.BRANCH_NAME}"
                    git url: 'https://github.com/your-repo/your-project.git', branch: env.BRANCH_NAME
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building branch: ${env.BRANCH_NAME}"
                    withMaven(maven: 'Maven-3.9.0') {
                        sh "'${MAVEN_HOME}/bin/mvn' clean package"
                    }
                }
            }
        }

        stage('Integration Test') {
            steps {
                script {
                    echo "Running integration tests on branch: ${env.BRANCH_NAME}"
                    withMaven(maven: 'Maven-3.9.0') {
                        sh "'${MAVEN_HOME}/bin/mvn' verify"
                    }
                }
            }
        }

        stage('Static Analysis') {
            steps {
                script {
                    echo "Performing static analysis on branch: ${env.BRANCH_NAME}"
                    sh "'${MAVEN_HOME}/bin/mvn' pmd:pmd"
                }
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch 'staging'
                }
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo "Deploying to production from branch: ${env.BRANCH_NAME}"
                        sh './deploy-prod.sh'
                    } else if (env.BRANCH_NAME == 'staging') {
                        echo "Deploying to staging from branch: ${env.BRANCH_NAME}"
                        sh './deploy-staging.sh'
                    }
                }
            }
        }
    }

    post {
        always {
            node('any') { // Specify 'any' or your specific label here
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
                junit '**/target/test-classes/*.xml'
            }
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
