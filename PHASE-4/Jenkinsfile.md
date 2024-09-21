```
pipeline {
    agent any
    
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git credentialsId: 'git-credential', url: 'https://github.com/etechsconsulting/java-maven-app.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar-credential') {
                 sh *******
                }
            }
        }
        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: False, credentailsId: 'put your token'
            }
        }
        stage('Build Artifact') {
            steps {
                sh "mvn package"
            }
        }
        stage('Publish To Nexus') {
            steps {***
                sh "mvn deploy"
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                            sh "docker build -t ebonje/java-maven-app:1.0 ."
                   }    
                }     
            } 
        }
        stage('Docker Scan Image') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ebonje/java-maven-app:1.0 "
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh "docker push ebonje/java-maven-app:1.0"
                    }
                } 
            }
        }
        stage('Deploy To Kubernetes') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', 
                   namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.8.146:6443') {
               }         sh "kubectl apply -f deployment-service.yaml"
            }
        }
        
        stage('Verify the Deploy') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', 
                   namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.8.146:6443') {
                   sh "kubectl  get pod -n webapps"
                   sh "kubectl get svc -n webapps"
            }
        }
    }
}

