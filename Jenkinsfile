def CONTAINER_NAME="jenkins-pipeline"
def CONTAINER_TAG="latest"


pipeline {
    agent none
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            agent {
                docker {
                image 'maven:3.8.1-adoptopenjdk-11'
                args '-v /root/.m2:/root/.m2'
                 }
           }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            agent {
              docker {
                   image 'maven:3.8.1-adoptopenjdk-11'
                   args '-v /root/.m2:/root/.m2'
                 }
              }
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Image Build'){
            agent any
             steps {
                sh "docker --version"
                echo "Image build complete"
             }
        }
        stage('Deliver') { 
                agent {
            docker {
                image 'maven:3.8.1-adoptopenjdk-11'
                args '-v /root/.m2:/root/.m2'
            }
        }
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
       
    }
}
