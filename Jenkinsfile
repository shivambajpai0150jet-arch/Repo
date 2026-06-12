pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t petclinic .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop petclinic || true
                docker rm petclinic || true

                docker run -d \
                -p 8081:8080 \
                --name petclinic \
                petclinic
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully'
        }

        failure {
            echo 'Deployment failed'
        }
    }
}
