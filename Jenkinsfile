


pipeline {
    agent any
    

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('sonar-token')
        SONAR_ORGANIZATION = 'jenkins-project-12345-ci'
        SONAR_PROJECT_KEY = 'jenkins-project-12345-ci_ci-jenkins'
    }

    stages {
        
        stage('Code-Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
  -Dsonar.organization=jenkins-project-12345-ci \
  -Dsonar.projectKey=jenkins-project-12345-ci_ci-jenkins \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io '''
                }
            }
        }
       
        
      
       stage('Docker Build And Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-cred') {
                        def buildNumber = env.BUILD_NUMBER ?: '1'
                        def image = docker.build("jason19940518/crud-123:latest")
                        image.push()
                    }
                }
            }
        }
    
       
        stage('Deploy To EC2') {
            steps {
                script {
                        sh 'docker rm -f $(docker ps -q) || true'
                        sh 'docker run -d -p 3000:3000 jason19940518/crud-123:latest'
                        
                    
                }
            }
        }
        
}
}
