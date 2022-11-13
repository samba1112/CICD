pipeline {
    agent any
    environment {
        MYWORKSPACE = ""
    }
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
                script{
                MYWORKSPACE = WORKSPACE
                git branch: 'main', url: 'https://github.com/samba1112/CICD.git'
                
                sh 'mvn -B -DskipTests clean package'
            }}
        }
        stage('Dockerbuild and Push')
        { 
            steps
            {
            script
            { 
             dir(MYWORKSPACE){
              sh 'docker build . -t samba1112/cicd-project:$BUILD_NUMBER'
              withCredentials([string(credentialsId: 'docker-password', variable: 'password')]) {
                   sh 'docker login -u samba1112 -p $password'
                   sh 'docker push samba1112/cicd-project:$BUILD_NUMBER'
                }
            }
            }
            }          
        }
        }
    }
