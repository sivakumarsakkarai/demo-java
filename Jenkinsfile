pipeline {
    environment {
    registry = "sivakumarsakkarai/demo-java"
    registryCredential = "dockerhub"
    }
    agent any    
    stages{
        stage("build & SonarQube analysis"){
            steps{                
                withSonarQubeEnv('df-sonar'){
                    sh 'mvn clean install'
                } 

            }
        }
        stage("Quality Gate"){
            steps{
                timeout(time: 10, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                    }
                }
            }
        stage("Building Image"){
            steps{
                script {
                docker.build registry + ":$BUILD_NUMBER"              
                }
            }
        }
    }
}
