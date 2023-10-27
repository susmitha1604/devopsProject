pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
               git branch: 'main', url: 'https://github.com/susmitha1604/devopsProject.git'
            }
        }
        stage('Docker Build'){
            steps{
                sh "docker build . -t susmithan/php-website"
            }
        }
        stage('DockerHub Push'){
            steps{
                
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerPwd')]) {
                      sh "docker login -u susmithan -p ${dockerPwd}"
                }
                
                sh "docker push susmithan/php-website "
            }
        }
         stage('Install docker and its dependencies and run contianer') {
            steps {
               ansiblePlaybook credentialsId: 'test-agent', installation: 'ansible', inventory: 'servers.inv', playbook: 'deployment-docker.yml'
            }
        }
    }
}

