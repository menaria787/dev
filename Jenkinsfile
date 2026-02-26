pipeline {
    agent any
        
    stages{
        stage("code checkouts"){
            steps{
                git branch: 'main', url: 'https://github.com/menaria787/dev.git'
            }
        }
        stage("image build"){
            steps{
                sh 'docker image build -t menaria787/todoapp:v$BUILD_ID .'
                sh 'docker image tag menaria787/todoapp:v$BUILD_ID menaria787/todoapp:latest'
                }
        }
        stage("image Push") 
    {
     steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'user')]) {
          sh "docker login -u ${user} -p ${password}"
          sh 'docker image push menaria787/todoapp:v$BUILD_ID'
          sh 'docker image push menaria787/todoapp:latest'
          sh 'docker image rmi menaria787/todoapp:v$BUILD_ID menaria787/todoapp:latest'
        }
   }
}
 stage("docker container creating"){
    steps {
        sh '''
            # Stop and remove existing container if it exists
            if [ $(docker ps -aq -f name=todoapp) ]; then
                echo "Stopping and removing existing container..."
                docker rm -f todoapp
            fi

            # Run new container
            echo "Starting new container..."
            docker run -d --name todoapp -p 80:3000 menaria787/todoapp:latest
        '''
    }
 }

    }
}


