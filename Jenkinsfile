pipeline {
    agent any

    environment {
        PATH = "${PATH}:/opt/apache-maven-3.9.10/bin"
    }

    stages {
        stage('Get Code') {
            steps {
                echo 'Cloning the Git repository...'
                git 'https://github.com/Murtuza7007/maven-web-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Starting the Maven build...'
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('Sonar-Server-7.8') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Code Deploy') {
            steps {
                echo 'Deploying application to Tomcat server...'
                sshagent(['Tomcat-Server']) {
                    sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@13.204.86.253:/home/ec2-user/apache-tomcat-9.0.106/webapps/'
                }
            }
        }
    }
}
