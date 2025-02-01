pipeline{
    agent any
    environment{
        DOCKER_HUB_USER = 'maheshgowdamg25'
        DOCKER_HUB_PASS = 'Mahi@2001' 
    }
    stages{
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
                        '''
                    }
                }
            }
         }
        
        stage('pull'){
            steps{
                sh 'docker pull nginx'
            }
        }
        stage('run'){
            steps{
                sh 'docker run -it -d --name app -p 8000:80 nginx'
            }
        }
        stage('snapshot'){
            steps{
                sh 'docker commit app mahesh'
            }
        }
        stage('tag'){
            steps{
                sh 'docker tag mahesh maheshgowdamg25/mock'
            }
        }

         stage('Login to Docker Hub') {
            steps {
                sh 'echo "$DOCKER_HUB_PASS" | docker login -u "$DOCKER_HUB_USER" --password-stdin'
            }
        }

        stage('push'){
            steps{
                sh 'docker push maheshgowdamg25/mock'
            }
        }
        
    }
    post{
        always{
            sh 'docker rmi nginx'
        }
        success{
            sh 'docker rm -f app'
        }
    }
    
}
