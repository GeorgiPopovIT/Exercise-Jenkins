pipeline {
    agent any
    stages {
        stage('Repo checkout') {
            steps {
                script{
                    checkout scm
                }
            }
        }

        stage('NPM install') {
            steps {
                bat "npm install"
            }
        }
        
        stage('Run integration tests') {
            steps {
               bat "npm run test"
            }
        }

        stage('Build Image') {
            steps {
                withcredentials([usernamePassword(credentialsId: '5b044471-d530-4fa0-ac25-dbc4f321c93e', passwordVariable: 'pass')]) {
                bat "docker build -t georges1912/student-registry-app:1.0.0 .
                    docker login --username georges1912 --password %pass%
                    docker push georges1912/student-registry-app:1.0.0"
                }
            }
        }

        stage('Deploy') {
            steps {
                withcredentials([usernamePassword(credentialsId: '5b044471-d530-4fa0-ac25-dbc4f321c93e', passwordVariable: 'pass')]) {
                bat "docker login --username georges1912 --password %pass%
                    docker pull georges1912/student-registry-app:1.0.0
                    docker-compose -f docker-compose.yml up -d"
                }
            }
        }
    }
}