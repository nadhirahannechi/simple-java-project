pipeline {
    agent any
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build("nadhirahannechi/java-pipeline:${TAG}")
                }
            }
        }
	    stage('Pushing Docker Image to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        docker.image("nadhirahannechi/java-pipeline:${TAG}").push()
                        docker.image("nadhirahannechi/java-pipeline:${TAG}").push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                sh "docker stop java-pipeline | true"
                sh "docker rm java-pipeline | true"
                sh "docker run --name java-pipeline -d -p 9004:8080 nadhirahannechi/java-pipeline:${TAG}"
            }
        }
    }
}
