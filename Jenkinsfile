pipeline {
    agent any  // Runs on the master node or any available agent

    environment {
        MAVEN_HOME = tool name: 'Maven-3.9.2'  // Ensure this tool is configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Checking out branch: ${env.BRANCH_NAME}"
                    git url: 'https://github.com/Nency-Ravaliya/Day14.git', branch: env.BRANCH_NAME
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building branch: ${env.BRANCH_NAME}"
                    withMaven(maven: 'Maven-3.9.2') {
                        sh "'${MAVEN_HOME}/bin/mvn' clean package"
                    }
                }
            }
        }

        stage('Integration Test') {
            steps {
                script {
                    echo "Running integration tests on branch: ${env.BRANCH_NAME}"
                    withMaven(maven: 'Maven-3.9.2') {
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
