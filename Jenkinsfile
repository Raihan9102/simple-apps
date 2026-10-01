pipeline {
    agent { label 'host1-raihan' }
  
    stages {
       SONAR_HOST=credentials('sonar-host')
       SONAR_TOKEN=credentials('sonar-token')
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
                -Dsonar.host.url=${SONAR_HOST} \
                -Dsonar.token=${SONAR_TOKEN}
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