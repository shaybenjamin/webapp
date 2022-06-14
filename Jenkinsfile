pipeline {
    agent { node { label 'linux' } }
    environment {
        DOCKERHUB_CREDENTIALS=credentials('DockerhubCreds')
    }
    
    stages {
        stage("build docker") {
            steps {
                sh "pwd"
                sh "whoami"
                sh "docker build -t shayben/webapp:latest ."
            }
        }
        stage("verify dockers") {
            steps {
                sh "docker images"
            }
        }
         stage("promote image") {
            steps {
                sh "docker build -t shayben/webapp:verified ."
            }
        }
        stage("login") {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage("push") {
            steps {
                sh 'docker push shayben/webapp:verified'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}

