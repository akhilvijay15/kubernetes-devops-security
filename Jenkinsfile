pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        } 
      
      stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push') {
      steps {
         
                sh 'docker build -t siddharth67/numeric-app:""$GIT_COMMIT"" .'
            }
            
        }
        stage('Push'){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPassword')]) {
          
          sh "docker login -u ${env.dockerHUbuser} -p ${env.dockerHubpassword}"
          sh 'docker push akhilvijay15/numeric-app:tagname''

        }
      }
    }
  }
}

 
