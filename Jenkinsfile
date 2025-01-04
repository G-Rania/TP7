pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew build"
                archiveArtifacts '**/build/libs/*.jar'
                //junit '**/build/test-results/**/*.xml'
                echo 'Testing & archiving..'
                echo 'Generating cucumber reports...'
                cucumber '**/build/reports/cucumber/*.html'
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
                     bat './gradlew assemble'
                     echo 'Generating Documentation...'
                     bat './gradlew javadoc' // Génération de la documentation
                     archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
                     archiveArtifacts artifacts: '**/build/docs/javadoc/**', fingerprint: true
                 }
        }

    }
}