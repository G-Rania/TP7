pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                bat "./gradlew test"
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
                    bat './gradlew sonarqube'
                }
            }
         }

         stage('Code Quality') {
             steps {
                 script {
                     echo 'Checking SonarQube Quality Gates...'
                     timeout(time: 5, unit: 'MINUTES') {
                         def qg = waitForQualityGate()
                         if (qg.status != 'OK') {
                             error "Pipeline aborted due to failing Quality Gate: ${qg.status}"
                         }
                     }
                 }
             }
         }
    }
}