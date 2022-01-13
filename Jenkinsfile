pipeline {
    agent any

    stages {
        stage('pull project') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'e65a3e88-fdd0-44a4-a618-f1793a1e900c', url: 'git@github.com:LhjWhw/jenkins-demo.git']]])
            }
        }

        stage('build project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube代码审查') {
            steps{
                script {
                    scannerHome = tool 'sonarqube-scanner'
                }
                withSonarQubeEnv('sonarqube6.7.4') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
         }

        stage('publish project') {
            steps {
                deploy adapters: [tomcat8(credentialsId: '9018a1df-f69b-4566-b757-3367d6c5fefa', path: '', url: 'http://124.223.5.50:8081/')], contextPath: null, war: 'target/*.war'
            }
        }
    }
}
