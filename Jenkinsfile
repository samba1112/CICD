pipeline {
    agent any
    environment {
        MYWORKSPACE = ""
        tag = "tag"
        new_value = "$BUILD_NUMBER"
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
              
              sh "pwd && ls"
              sh 'docker build . -t samba1112/cicd-project:$BUILD_NUMBER'
              withCredentials([string(credentialsId: 'docker-password', variable: 'pass')]) {
                  sh "echo $pass"
                   sh 'docker login -u samba1112 -p $pass'
                   sh 'docker push samba1112/cicd-project:$BUILD_NUMBER'
                }
            }}
            }          
        }
        stage('Helm value update')
        { steps{
            script{
            dir(MYWORKSPACE)
            {
                sh """
                cd CD/sample
                sed -r "s/^(\\s*${tag}\\s*:\\s*).*/\\1${new_value}/" -i values.yaml
                git config user.email "sambasiva1112@gamil.com"
                git config user.name "samba112"
                git add .
                git commit -m "Changed image tag"
                git push https://ghp_LE529LbIPQ50TXJPyt7thzQa4ojLMV1STfJ@github.com/samba1112/CICD.git main
                """
            }
            }
        }
        }
 }
    }
