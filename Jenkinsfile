      pipeline {
          environment {
          registry = 'sivakumarsakkarai/demo-java'
          registryCredential = 'dockerhub'
          dockerImage=''
          }
        agent any    
        stages {
          stage("build & SonarQube analysis") {            
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
                sh 'bin/build'  
                sh 'docker build -t registry + ":$BUILD_NUMBER" .'
            }
          }  
        }
      }
