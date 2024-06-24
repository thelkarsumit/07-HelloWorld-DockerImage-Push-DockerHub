pipeline {
    agent any
    
    environment {
        SCANNER_HOME=tool 'SonarQube-Scanner'
      }
    
  tools {
        maven 'Maven'
    }
    
 stages {

        stage('Test') {
            steps {
                // Define steps for the Test stage
                echo 'Running tests...'
                sh  'mvn test'
            }
        }

        stage('Build') {
            steps {
                // Define steps for the Build stage
                echo 'Building the project...'
                sh 'mvn clean package'
            }
        }
     
        stage('sonarqube-analysis') {
            steps {
                withSonarQubeEnv('SonarQube-server') {
                 sh 'mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=HelloWorld-DockerImage-Push-GCP \
                      -Dsonar.host.url=http://35.228.47.4:9000 \
                      -Dsonar.login=sqp_0cbc01c8a34a6a1e5b3a7c3d66feee59c71b756d'
                }
            }
        }
        
    stage('Docker Build') {
            steps {
                script {
                    sh 'docker login -u sumitthelkar -p Docker@1234'
                    sh 'docker push sumitthelkar/my-app:latest'
                }
            }
        }
     
     stage('Run Docker Container') {
            steps {
                script {
                    // 3. Run the Container
                    sh 'docker run -d -p 8080:8080 sumitthelkar/my-app:latest'
                }
            }
        }
    }
    post {
        success {
            // Actions to take if the pipeline succeeds
            echo 'Pipeline succeeded!'
            // You can also send an email notification on success
            mail to: 'thelkarsc@gmail.com',
                 subject: "Pipeline Succeeded: ${currentBuild.fullDisplayName}",
                 body: "The pipeline ${env.BUILD_URL} has successfully completed."
        }
        failure {
            // Actions to take if the pipeline fails
            echo 'Pipeline failed!'
            // You can also send an email notification on failure
            mail to: 'thelkarsc@gmail.com',
                 subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                 body: "The pipeline ${env.BUILD_URL} has failed. Check the logs for details."
        }
    }
}
