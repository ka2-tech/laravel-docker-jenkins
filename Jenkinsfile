pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'develop', credentialsId: 'github', url: 'https://github.com/khaitk/laravel-docker-jenkins.git'
            }
        }

        stage('Build Docker Image') {         
            steps{                
                sh 'docker build -t khraiteka/laravel-jenkins-docker:$BUILD_NUMBER .'           
                echo 'Build Image Completed'                
            }           
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push khraiteka/laravel-jenkins-docker:$BUILD_NUMBER'
                echo 'Push Image Completed' 
            }
        }
    }
    post {
        failure {
              mail to: 'khaitkdev@gmail.com',
                subject: "FAILED: Build ${env.JOB_NAME}", 
                body: "Build failed ${env.JOB_NAME} build no: ${env.BUILD_NUMBER}.\n\nView the log at:\n ${env.BUILD_URL}\n\nURL:\n${env.RUN_DISPLAY_URL}"
        }
    
    success{
            mail to: 'khaitkdev@gmail.com',
                subject: "SUCCESSFUL: Build ${env.JOB_NAME}", 
                body: "Build Successful ${env.JOB_NAME} build no: ${env.BUILD_NUMBER}\n\nView the log at:\n ${env.BUILD_URL}\n\nURL:\n${env.RUN_DISPLAY_URL}"
        }
        
        aborted{
            mail to: 'khaitkdev@gmail.com',
                subject: "ABORTED: Build ${env.JOB_NAME}", 
                body: "Build was aborted ${env.JOB_NAME} build no: ${env.BUILD_NUMBER}\n\nView the log at:\n ${env.BUILD_URL}\n\nURL:\n${env.RUN_DISPLAY_URL}"
        }
    }
}