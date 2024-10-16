pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Smashkaad2/TallerOpsDev'
            }
        }

        stage('Lint') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install pylint
                    pylint **/*.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t talleropsdev-app .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker run -d -p 5000:5000 talleropsdev-app
                '''
            }
        }
    }

    post {
        always {
            sh '''
                docker rm $(docker ps -a -q) || true
                docker rmi talleropsdev-app || true
            '''
        }
        success {
            echo 'Pipeline ejecutado con éxito.'
        }
        failure {
            echo 'Pipeline falló.'
        }
    }
}
