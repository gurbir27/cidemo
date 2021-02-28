pipeline {
    agent {
        docker {
            image "maven:3.6.3-jdk-8"
        }
    }
    triggers {
        pollSCM("* * * * *")
    }
    stages {
        stage('Prepration') {
            steps {
                cleanWs()
            }
        }
        stage('Declarative CheckOut') {
            steps {
                sh "echo Checking Out code"
                checkout scm
            }
        }
        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Unit Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Publish Test Result') {
            steps {
                junit '**/surefire-reports/*.xml'
            }
        }
        stage('Code Quality Check') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar') {
                    sh "mvn sonar:sonar"
                    // some block
                }
            }
        }

    }

    post {
        always {
            echo "Looks good"
        }
    }
}