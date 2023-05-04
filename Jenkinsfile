pipeline {
    agent any 
    environment {
        DOCKER_IMAGE_NAME = "Himanshu/train-schedule"
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
     stage('Build and push docker image') {
         steps {
                
                sh 'sudo docker build -t Himanshu/train-schedule:$BUILD_NUMBER .'
                sh "sudo docker login -u ${DOCKER_COMMON_CREDS_USR} -p ${DOCKER_COMMON_CREDS_PSW}"
                sh 'sudo docker push Himanshu/train-schedule:$BUILD_NUMBER'
                }
        }  
      stage ('Kubernetes deployment') {
         steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl set image deployment train-schedule train-schedule=Himanshu/train-schedule:$BUILD_NUMBER'   
            }  
        }
    }
}
