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

         stage('Deploy') {
            steps {
                echo 'Deploying with Maven...'
                bat './gradlew publish'
            }
         }

         stage('Notification') {
             steps {
                 script {
                     currentBuild.result = currentBuild.result ?: 'SUCCESS'
                     if (currentBuild.result == 'SUCCESS') {
                         echo 'Sending success notification...'
                         mail to: 'lr_gueddouche@esi.dz',
                              subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                              body: "The build and deployment for ${env.JOB_NAME} #${env.BUILD_NUMBER} was successful."
                         //slackSend(channel: SLACK_CHANNEL, message: "Deployment Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                     } else {
                         echo 'Sending failure notification...'
                         mail to: 'lr_gueddouche@esi.dz',
                              subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                              body: "The build for ${env.JOB_NAME} #${env.BUILD_NUMBER} failed. Check the logs for details."
                         //slackSend(channel: SLACK_CHANNEL, message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                     }
                 }
             }
         }


    }
}