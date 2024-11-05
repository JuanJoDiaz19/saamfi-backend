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
                sh 'mvn install:install-file -Dfile="./ojdbc6-11.2.0.3.jar" -DgroupId=oracle -DartifactId=ojdbc6 -Dversion=11.2.0.3 -Dpackaging=jar'
                sh 'mvn clean install -DskipTests -U'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Add Dokku Host Key') {
            steps {
                sh '''
                    mkdir -p ~/.ssh
                    ssh-keyscan -H helpme-god-juanjo.centralus.cloudapp.azure.com >> ~/.ssh/known_hosts
                '''
            }
        }

        stage('Deploy to Dokku') {
            steps {
                sshagent(['ssh-dokku']) {
                    sh "git remote remove dokku || true"
                    sh "git remote add dokku dokku@helpme-god-juanjo.centralus.cloudapp.azure.com:saamfi2-backend || true"
                    sh "git push dokku main -f"
                }
            }
        }
    }

    // post {
    //     always {
    //         junit '**/target/surefire-reports/*.xml'
    //         jacoco execPattern: '**/target/*.exec'
    //         cleanWs() // Cleans up the workspace after build completion
    //     }
    // }
}
