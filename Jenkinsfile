pipeline {
    agent any

    environment {
      // example https://github.com/beatXD/world-country#main 
      gitlab_endpoint = ''

      image_name = 'image_name'
      service_name = 'service_name'

      port = '3000:3000'

      shell_address = 'shell_address'
      shell_password = 'shell_password'
    }

    stages {
        stage('Building...') {
            steps {
                sh 'docker build -f Dockerfile -t $image_name $gitlab_endpoint'
            }
        }

        stage('Pushing...') {
            steps {
                sh 'docker push $image_name'
            }
        }

        stage('Deploy Cleaning...') {
            steps {
                sh 'sshpass -p $shell_password ssh $shell_address docker stop $service_name || true'
                sh 'sshpass -p $shell_password ssh $shell_address docker system prune --force --all || true'
            }
        }

        stage('Deploy Pulling...') {
            steps {
                sh 'sshpass -p $shell_password ssh $shell_address docker pull $image_name'
            }
        }


        stage('Deploy Updating...') {
            steps {
                sh 'sshpass -p $shell_password ssh $shell_address docker run -d --restart=always --name $service_name -p:$port $image_name'
            }
        }

    }
}