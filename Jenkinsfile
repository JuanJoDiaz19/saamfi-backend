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
                sh '''
                    mvn -B install:install-file \
                        -Dfile="./ojdbc6-11.2.0.3.jar" \
                        -DgroupId=com.oracle.database.jdbc \
                        -DartifactId=ojdbc6 \
                        -Dversion=11.2.0.3 \
                        -Dpackaging=jar
                '''
                sh 'mvn -B clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -B test'
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
