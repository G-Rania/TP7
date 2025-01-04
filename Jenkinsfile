pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew test"
                archiveArtifacts  '**/build/libs/*.jar'
                echo 'Testing & archiving..'
                echo 'Generating cucumber reports...'
                cucumber '**/build/reports/cucumber/*.html'
            }
        }

         stage('Code Analysis') {
                    steps {
                        echo 'Starting Code Analysis with SonarQube...'
                        withSonarQubeEnv('Sonar') {
                            bat './gradlew sonarqube'
                        }
                    }
                }
    }
}