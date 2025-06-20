pipeline {
    agent any
    
    environment {
        DOCKER_COMPOSE = 'docker-compose'
        FRONTEND_DIR = 'LMS-Frontend'
        BACKEND_DIR = 'LMS-Backend'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh '''
                    npm install
                    cd ${FRONTEND_DIR} && npm install
                    cd ../${BACKEND_DIR} && pip install -r requirements.txt
                '''
            }
        }
        
        stage('Build Frontend') {
            steps {
                sh '''
                    cd ${FRONTEND_DIR}
                    npm run build
                '''
            }
        }
        
        stage('Test Backend') {
            steps {
                sh '''
                    cd ${BACKEND_DIR}
                    python manage.py test
                '''
            }
        }
        
        stage('Build Docker Images') {
            steps {
                sh '''
                    ${DOCKER_COMPOSE} build
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                    ${DOCKER_COMPOSE} down
                    ${DOCKER_COMPOSE} up -d
                '''
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
