

pipeline {
    agent {label 'gcp_instance'}
    

    stages 
    {
        stage('git checkout') 
        {
            steps {
   
                git 'https://github.com/nohaaltayeb/application_nodejs.git'
            }
        }
        
        stage('build') {
            steps {
                sh 'docker build -f dockerfile . -t nohaaltayeb/nodejs:latest'
            }
        }
        
        stage('artifact') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'username')]){
                sh 'sudo docker login -u ${username} -p ${pass}'
                sh 'sudo docker push nohaaltayeb/nodejs:latest'
            }
           }
        }
        stage('git app') {
            steps {
                git 'https://github.com/nohaaltayeb/application_nodejs.git'
            }
        }
        stage('deploy') {
            steps 
            {

                //sh 'kubectl create namespace my-app'
                sh 'kubectl apply -f ./k8s_deployment/app_deploy.yml -n my-app'
                sh 'kubectl apply -f ./k8s_deployment/service_app.yml -n my-app'
                
            }
           }
        }
        

   }
