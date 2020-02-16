pipeline {
    agent any
    stages{
        stage("Git CheckOut"){
            steps{
                credentialsId: 'github', url: 'https://github.com/sivakumarsakkarai/demo-java.git'
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
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK'){
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }


            }
        }
    }
}
