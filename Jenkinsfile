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
                 sh "sudo apt-get update"
                 sh "sudo apt-get install ca-certificates curl gnupg lsb-release"
                 sh "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg"
                 sh "echo \"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable"
                 sh "sudo tee /etc/apt/sources.list.d/docker.list > /dev/null"
                 sh "sudo apt-get update"
                 sh "sudo apt-get install docker-ce docker-ce-cli containerd.io"
                 sh "docker build -t jenkins-pipeline:latest  -t jenkins-pipeline --pull --no-cache ."
                 echo "Image build complete"
             }
         }
             }
            }
          
