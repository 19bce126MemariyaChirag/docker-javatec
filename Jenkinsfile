pipeline {
    agent any
    tools{
        maven '3_9_0'
    }
    tools{
           maven '3_9_1'
       }
        stages{
            stage('build'){
                steps{
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/19bce126MemariyaChirag/docker-javatec']])
                    sh 'mvn clean install'
                }
            }
            stage('build docker image'){
                steps{
                    script{
                        sh 'docker build -t memariyachirag126/devops-integration . '
                    }
                }
            }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-hub-2', variable: 'docker-hub-2')]) {
//                     withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u memariyachirag126 -p ${docker-hub-2}'

}
                   sh 'docker push memariyachirag126/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
