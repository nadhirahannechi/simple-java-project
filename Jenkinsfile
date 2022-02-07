pipeline {
     agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11'
                    args '-v /root/.m2:/root/.m2'
                }
     }
    options {
                skipStagesAfterUnstable()
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
                 stage("build image"){
             steps {
                 sh "docker build -t jenkins-pipeline:latest  -t jenkins-pipeline --pull --no-cache ."
                 echo "Image build complete"
             }
         }
             }
            }
          
