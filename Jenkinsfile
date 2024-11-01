pipeline {
    agent any

    tools {
        // Specify the JDK and Maven versions installed in Jenkins
        jdk 'OpenJDK-17'
        maven 'maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                git url: 'https://github.com/JuanJoDiaz19/saamfi-backend.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                
                sh '''
                    mvn install:install-file \
                        -Dfile="./ojdbc6-11.2.0.3.jar" \
                        -DgroupId=com.oracle.database.jdbc \
                        -DartifactId=ojdbc6 \
                        -Dversion=11.2.0.3 \
                        -Dpackaging=jar
                '''
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            jacoco execPattern: '**/target/*.exec'
        }
    }
}
