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
        stage('Hello') {
            steps {
                echo 'Hello World'
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
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}

