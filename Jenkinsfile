pipeline {
    agent { label 'host1-raihan' }
    SONAR-HOST=credentials('sonar-host')
    SONAR-TOKEN=credentials('sonar-token')


    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Raihan9102/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=${SONAR-HOST} \
                -Dsonar.token=${SONAR-TOKEN}
                '''
            }
        }
        
        stage('Delivery') {
            steps {
                
                input message: 'apakah sudah yakin untuk deploy ke production?', submitter: 'Deploy sekarang!'
                
            }
        }

        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        
    }
}