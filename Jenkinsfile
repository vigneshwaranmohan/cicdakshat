pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git credentialsId: 'githubcred', url: 'https://github.com/vigneshwaranmohan/cicdakshat'
               sh 'mvn clean install'
               sh 'mvn package'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t vikeenanee/oct2026sicdproject:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push vikeenanee/oct2026sicdproject:v1'
                }
            }
        }
        
        
        stage('Deploy to k8s'){

            steps{
                script{
                     kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
