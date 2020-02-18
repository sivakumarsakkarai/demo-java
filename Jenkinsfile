pipeline {
    environment{
        PATH= "/opt/maven/bin:$PATH"
    }
    agent any
    stages{
        stage("Git CheckOut"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'GitHubSK', url: 'https://github.com/sivakumarsakkarai/demo-java.git']]])
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
        stage("Building Image"){
            steps{
                sh 'docker build -f "Dockerfile" -t sivakumarsakkarai/demo-java:$BUILD_NUMBER .'
                
            }
        }
    }
}
