pipeline {
    agent none
    options {
                skipStagesAfterUnstable()
            }
     stages {
        stage("build and test the project") {
            agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            
            stages {
                stage('Build') {
                    steps {
                        sh 'mvn -B -DskipTests clean package'
                    }
                }
                stage('Test') {
                    steps {
                        sh 'mvn test'
                    }
                    post {
                        always {
                            junit 'target/surefire-reports/*.xml'
                        }
                    }
                }
                stage('Deliver') { 
                    steps {
                        sh './jenkins/scripts/deliver.sh' 
                    }
                }
             }
            }
         stage("build image"){
             agent any
             steps {
                 sh 'docker --version'
             }
         }
     }
}
             
