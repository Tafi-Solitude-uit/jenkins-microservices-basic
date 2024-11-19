pipeline {
    agent any
    
    environment {
        SCANNER_HOME= tool 'SonarScanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Tafi-Solitude-uit/jenkins-microservices-basic.git'
            }
        }
        
        stage("Sonar Analysis") {
            steps {
                sh ''' $SCANNER_HOME/bin/sonar-scanner \
                           -Dsonar.host.url=http://192.168.106.39:9000 \
                           -Dsonar.login=<sonar-token> \
                           -Dsonar.projectName=to-do-app \
                           -Dsonar.projectKey=to-do-app \
                           -Dsonar.sources=.
                '''
            }
        }
        
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Docker Build & Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: '<credential của docker>') {
                        sh "docker build -t todoapp:latest -f backend/Dockerfile . "
                        sh "docker tag todoapp:latest anhtaiht/todoapp:latest "
                        sh "docker push anhtaiht/todoapp:latest"
                 }
               }
            }
        }
        
        stage('Trivy Docker Scan') {
            steps {
                sh "trivy image anhtaiht/todoapp:latest"
            }
        }
        
        stage('Deploy to Docker') {
            steps {
              script{
                  withDockerRegistry(credentialsId: '<credential của docker>') {
                    sh "docker run -d --name to-do-app -p 4000:4000 anhtaiht/todoapp:latest "
                  }
              }
            }
        }
    }
}
