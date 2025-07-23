pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'master', url: 'https://github.com/Aarav2411/Aperture-Technology-co.git'
            }
        }

        stage('List Website Files') {
            steps {
                echo "Files in the aperture-homepage directory:"
                sh 'ls -lh aperture-homepage/'
            }
        }

        stage('Archive Website Files') {
            steps {
                archiveArtifacts artifacts: 'aperture-homepage/**', fingerprint: true
            }
        }

        stage('Deploy to Nginx Docker') {
            steps {
                echo 'Deploying to local Nginx container...'
                sh '''
                docker rm -f static-nginx || true
                docker run -d --name static-nginx \
                  -v $WORKSPACE/aperture-homepage:/usr/share/nginx/html:ro \
                  -p 8080:80 nginx
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
