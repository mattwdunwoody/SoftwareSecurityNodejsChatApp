pipeline {
    agent none
    stages {
        stage('CLONE GIT REPOSITORY') {
            agent {
                label 'appserver-agent'
            }
            steps {
                checkout scm
            }
        }
 
        stage('BUILD-AND-TAG') {
            agent {
                label 'appserver-agent'
            }
            steps {
                script {
                    def app = docker.build("widmatg/cybr-chat-app-img")
                    app.tag("latest")
                }
            }
        }
 
        stage('POST-TO-DOCKERHUB') {    
            agent {
                label 'appserver-agent'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'cybr-3120') {
                        def app = docker.image("widmatg/cybr-chat-app-img")
                        app.push("latest")
                    }
                }
            }
        }
 
        stage('DEPLOYMENT') {    
            agent {
                label 'appserver-agent'
            }
            steps {
                sh "docker-compose down"
                sh "docker-compose up -d"   
            }
        }
    }
}
