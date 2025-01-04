pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew build"
                archiveArtifacts  '**/build/libs/*.jar'
                echo 'Testing..'
                echo 'Generating Cucumber Reports...'
                cucumber '**/build/reports/cucumber/*.html'
            }
        }
    }
}