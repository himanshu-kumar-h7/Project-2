pipeline {
    agent any
    environment {
        //be sure to replace "bhavukm" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "shubha123anindya/train-schedule"
        DOCKER_COMMON_CREDS = credentials('dockerhub')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        /*
        stage ('build and push docker image') {
            steps {
                sh 'sudo docker build -t shubha123anindya/train-schedule:$BUILD_NUMBER .'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "sudo docker login -u ${env.user} -p ${env.pass}"
                    sh 'sudo docker push shubha123anindya/train-schedule:$BUILD_NUMBER'
                }
            }
        }
        */
        
        stage('Build and push docker image') {
            
            steps {
                /*
                script {
                    app = sudo docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
                */
          
                sh 'sudo docker build -t shubha123anindya/train-schedule:$BUILD_NUMBER .'
                
               // withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "sudo docker login -u ${DOCKER_COMMON_CREDS_USR} -p ${DOCKER_COMMON_CREDS_PSW}"
                    sh 'sudo docker push shubha123anindya/train-schedule:$BUILD_NUMBER'
                //}
                
                
                }
        }
        
        
        /*
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        */
        
        
        stage ('kubernetes deployment') {
            steps {
               // sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl set image deployment train-schedule train-schedule=shubha123anindya/train-schedule:$BUILD_NUMBER'
                
            }
        }
        
        
        
        
        
        
        /*
        stage('CanaryDeploy') {
            when {
                branch 'master'
            }
            environment { 
                CANARY_REPLICAS = 1
            }
            steps {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
            }
        }
        
        
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            environment { 
                CANARY_REPLICAS = 0
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }
        
        */
    }
}
