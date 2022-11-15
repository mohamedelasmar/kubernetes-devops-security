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
     stage('Docker Build and Push') {
            steps {
              sh 'printenv'
              sh 'docker build -t mohamedelasmar/numeric-app:""$GIT-COMMIT"" .'
              sh 'docker push mohamedelasmar/numeric-app:""$GIT-COMMIT""'
            }
        }  
    
    }
}
