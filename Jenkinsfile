      pipeline {
          environment {
          registry = "sivakumarsakkarai/demo-java"
          registryCredential = 'dockerhub'
          }
        agent none
        stages {
          stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('df-sonar') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
          stage("Building Image"){
            steps{                
                sh 'docker.build registry + ":$BUILD_NUMBER"'
            }
          }  
        }
      }
