pipeline {
    agent any
    environment {
        NEXUS_CREDS = credentials('nexus_creds')
        NEXUS_DOCKER_REPO = 'localhost:8082'
    }
    stages {
        stage('Building image') {
            steps {
                sh '''
          docker build -t mundose-pin1 -t $NEXUS_DOCKER_REPO/mundose-pin1:latest .
          '''
            }
        }
        stage('Run tests') {
            steps {
                sh "docker run mundose-pin1 npm test"
            }
        }
        stage('Docker Login') {
            steps {
                echo 'Nexus Docker Repository Login'
                script {
                    withCredentials([usernamePassword(credentialsId: 'nexus_creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh ' echo $PASS | docker login -u $USER --password-stdin $NEXUS_DOCKER_REPO'
                    }
                }
            }
        }
        stage('Docker Push') {
            steps {
                echo 'Pushing Image to docker hub'
                sh 'docker push $NEXUS_DOCKER_REPO/mundose-pin1:latest'
            }
        }
    }
}
