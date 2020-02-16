pipeline {
    agent any
    stages{
        stage("Git CheckOut"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sivakumarsakkarai/demo-java.git']]])
            }
        }
        stage("build & SonarQube analysis"){
            steps{
                withSonarQubeEnv('df-sonar'){
                    sh 'mvn clean package sonar:sonar'
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
        }
    }