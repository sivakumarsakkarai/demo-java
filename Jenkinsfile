pipeline {
    environment {
    registry = "sivakumarsakkarai/demo-java"
    registryCredential = "dockerhub"
    }
    agent any    
    stages{
        stage("build & SonarQube analysis"){
            steps{
                def mvnHome =  tool name: 'M2_HOME', type: 'maven'
                withSonarQubeEnv('df-sonar'){
                    sh "${mvnHome}/bin/mvn sonar:sonar"
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
                docker.build registry + ":$BUILD_NUMBER"              
            }
        }
    }
}
