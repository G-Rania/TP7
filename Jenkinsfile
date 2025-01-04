pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew build"
                archiveArtifacts  '**/build/libs/tesResults/*.jar'
                echo 'Testing..'
                echo 'Generating Cucumber Reports...'
                cucumber '**/build/libs/cucumber/*.json'
            }
        }
    }
}