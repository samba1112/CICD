pipeline {
    agent any
    stages {
        stage('Checkout && Build')
        {
        agent 
        {
            docker {
            image 'maven:3.5.4'
            args '-v /root/.m2:/root/.m2'
            }
        }
            steps{
                git branch: 'main', url: 'https://github.com/samba1112/CICD.git'
                
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Dockerbuild and Push')
        { 
            steps
            {
            script
            {
              sh 'docker build . -t samba1112/cicd-project:$BUILD_NUMBER'
              withCredentials([string(credentialsId: 'docker-password', variable: 'docker-password')]) {
                   sh 'docker login -u samba1112 -p $docker-password'
                   sh 'docker push samba1112/cicd-project:$BUILD_NUMBER'
                }
            }
            }          
        }
        }
    }
}
