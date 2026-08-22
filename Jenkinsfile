pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Code Analysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.token=YOUR_SONAR_TOKEN_HERE'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo-app .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker stop demo-app-container || true
                    docker rm demo-app-container || true
                    docker run -d --name demo-app-container -p 8081:8080 jenkins-demo-app
                '''
            }
        }
    }
}
