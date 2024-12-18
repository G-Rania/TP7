pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew test"
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