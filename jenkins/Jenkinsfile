pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'pip install pylint'
            }
        }

        stage('Run Pylint') {
            steps {
                sh 'pylint $(find . -name "*.py")'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t talleropsdev-app .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker run -d --name talleropsdev-app -p 8080:8080 talleropsdev-app'
            }
        }
    }

    post {
        always {
            sh 'docker rm -f talleropsdev-app || true'
        }
    }
}
