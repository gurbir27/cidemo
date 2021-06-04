pipeline{
    agent{
        docker{
            image "maven:3.8.1-jdk-11"
        }
    }
    triggers{
        pollSCM(" * * * * *")
    }
    stages{
        stage('Prep'){
            steps{
                cleanWs()
            }
        }
        stage('Declarative Checkout'){
            steps{
                sh "echo checking out code"
                checkout scm
            }
        }
        stage('Complie'){
            steps{
                sh "mvn clean compile"
            }
        }
        stage('Test'){
            steps{
                sh " mvn test "
            }
        }
        stage('Publish Test'){
            steps{
                junit '**/surefire-reports/*.xml'
            }
        }
        stage('Code quality Check'){
            steps{
                withSonarQubeEnv('sonar') {
                    sh "mvn sonar:sonar"
                } 
            }
        }
        
    }
}