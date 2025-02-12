pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'
            }
        } 
    
     stage('Unit Tests') {
            steps {
              sh "mvn test"
            }
        }  
    
    
     stage('SonarQube - SAST') {
              steps {
                   withSonarQubeEnv('SonarQube') {
                   sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=numeric-application"
                  
    }
  }
}
     stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                sh 'docker build -t mohamedelasmar/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push mohamedelasmar/numeric-app:""$GIT_COMMIT""'
            }
         }  
       }
    
    stage('K8 Deployment - DEV') {
            steps {
              withKubeConfig([credentialsId: "kubeconfig"]) {
                sh "sed -i 's#replace#mohamedelasmar/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                sh "kubectl apply -f k8s_deployment_service.yaml"
            }
         }  
       }
    }
}
