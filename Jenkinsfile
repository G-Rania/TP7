pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew build"
                archiveArtifacts  '**/build/libs/*.jar'
                echo 'Testing & archiving..'
                echo 'Generating cucumber reports...'
                cucumber '**/build/reports/cucumber/*.html'
            }
        }
    }
}