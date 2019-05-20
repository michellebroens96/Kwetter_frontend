pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Cleanup'){
            steps{
                sh '''
                docker rmi $(docker images -f 'dangling=true' -q) || true
                docker rmi $(docker images | sed 1,2d | awk '{print $3}') || true
                '''
            }
        }

	
        stage('Docker Build') {
            steps {
                sh 'docker build ./Kwetteren/frontend/. -t myjenkins:latest'
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                  sh 'docker login -u $USERNAME -p $PASSWORD'
                }
                sh 'docker push myjenkins:latest'
            }
        }
    }
}
