pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew test"
                archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
                echo 'Testing..'
            }
        }
        /*stage('Code Analysis') {
            steps {

            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }*/
    }
}