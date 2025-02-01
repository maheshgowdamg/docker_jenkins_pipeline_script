pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'maheshgowdamg25' 
        DOCKER_HUB_PASS = 'Mahi@2001'
    }
    
    stages {
        stage('Install Docker') {
            steps {
                script {
                    def dockerInstalled = sh(script: 'which docker', returnStatus: true) == 0
                    if (!dockerInstalled) {
                        sh '''
                        sudo yum update -y
                        sudo yum install -y docker
                        sudo systemctl start docker
                        sudo systemctl enable docker
                        sudo usermod -aG docker jenkins
                        sudo chmod 666 /var/run/docker.sock
                        '''
                    }
                }
            }
        }
        
        stage('Pull Image') {
            steps {
                sh 'docker pull nginx'
            }
        }
        
        stage('Run Container') {
            steps {
                sh 'docker run -it -d --name app -p 8000:80 nginx'
            }
        }
        
        stage('Create Image Snapshot') {
            steps {
                sh 'docker commit app maheshgowdamg25/mock:latest'
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                sh 'echo "$DOCKER_HUB_PASS" | docker login -u "$DOCKER_HUB_USER" --password-stdin'
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                sh 'docker push maheshgowdamg25/mock:latest'
            }
        }
    }
    
    post {
        always {
            sh 'docker rmi nginx || true'
        }
        
        success {
            sh 'docker rm -f app || true'
            sh 'docker rmi maheshgowdamg25/mock:latest || true'
        }
    }
}
