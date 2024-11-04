pipeline {
    agent any

    tools {
        jdk 'OpenJDK-17'
        maven 'maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/JuanJoDiaz19/saamfi-backend.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                
                sh '''mvn install:install-file -Dfile="./ojdbc6-11.2.0.3.jar" -DgroupId=oracle -DartifactId=ojdbc6 -Dversion=11.2.0.3 -Dpackaging=jar'''
                sh 'mvn clean install -DskipTests -U'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Dokku') {
            sshagent(['ssh-dokku']) { 
                steps {
                        sh "git remote add dokku dokku@ec2-18-191-161-171.us-east-2.compute.amazonaws.com:saamfi2-backend"
                    sh "git push dokku main"
                }
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            jacoco execPattern: '**/target/*.exec'
            cleanWs() // Cleans up the workspace after build completion
        }
    }
}
