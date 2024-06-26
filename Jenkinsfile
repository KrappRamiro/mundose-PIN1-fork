pipeline {
    agent any
    stages {
        stage('Building image') {
            steps {
                sh '''
          docker build -t mundose-pin1 -t krappramiro/mundose-pin1-fork:latest .
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
                echo 'Docker Registry Login'
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker_creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh ' echo $PASS | docker login -u $USER --password-stdin'
                    }
                }
            }
        }
        stage('Docker Push') {
            steps {
                echo 'Pushing Image to docker hub'
                sh 'docker push krappramiro/mundose-pin1-fork:latest'
            }
        }
    }
}
