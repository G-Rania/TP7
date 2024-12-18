pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew build"
                archiveArtifacts  '**/build/libs/*.jar'
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