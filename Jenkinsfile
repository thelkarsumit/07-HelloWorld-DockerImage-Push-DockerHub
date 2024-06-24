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
                      -Dsonar.projectKey=07-HelloWorld-DockerImage-Push-DockerHub \
                      -Dsonar.host.url=http://34.88.201.76:9000 \
                      -Dsonar.login=sqp_a756c13dbb19d2b6fcf9be7e410bd0bf0c8858c3'
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
