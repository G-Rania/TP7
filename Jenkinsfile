pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Testing & archiving..'
                bat "./gradlew test"
                //bat "./gradlew generateCucumberReports"
                //archiveArtifacts '**/build/libs/*.jar'
            }
        }

         stage('Code Analysis') {
            steps {
                echo 'Starting Code Analysis with SonarQube...'
                withSonarQubeEnv('Sonar') {
                    bat './gradlew sonar'
                }
            }
         }

         /*stage('Code Quality') {
             steps {
                 echo 'Checking SonarQube Quality Gates...'
                 waitForQualityGate abortPipeline: true
            }
         }*/

        stage('Build') {
                 steps {
                     echo 'Building the Project...'
                     bat './gradlew build'
                     echo 'Generating Documentation...'
                     bat './gradlew javadoc' // Génération de la documentation
                     archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
                     archiveArtifacts artifacts: '**/build/docs/javadoc/**', fingerprint: true
                 }
        }

    }
}