pipeline {
    
    agent {
        node 'jenkins-agent'
    }
    
    tools {
        jdk 'jdk17'
        maven 'mvn'
    }
    
    environment {
        // This assumes the JAVA_HOME and MAVEN_HOME environment variables are set
        JAVA_HOME = "${tool 'jdk17'}"
        MAVEN_HOME = "${tool 'mvn'}"
        PATH = "${env.MAVEN_HOME}/bin:${env.JAVA_HOME}/bin:${env.PATH}"
        //SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'jenkins-key', url: 'https://github.com/Dhana889/Multi-Tier-With-Database.git'
            }
        }
        stage ("Compile") {
            steps {
                sh "mvn clean compile"
            }
        }
        stage ("Test") {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
      	stage("Trivy FS Scanner") {
            steps {
                // Trivy file system scanner
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
    }
}