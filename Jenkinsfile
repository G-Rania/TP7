pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Testing & archiving..'
                bat "./gradlew test"
                archiveArtifacts '**/build/libs/*.jar'
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
                     def webhookUrl = 'https://hooks.slack.com/services/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'
                     def message_success = "Hello! This is a notification from Jenkins Pipeline. Case: Success"
                     def message_failure = "Hello! This is a notification from Jenkins Pipeline. Case: Failure"
                     currentBuild.result = currentBuild.result ?: 'SUCCESS'
                     if (currentBuild.result == 'SUCCESS') {
                         echo 'Sending success notifications...'
                         mail to: 'lr_gueddouche@esi.dz',
                              subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                              body: "The build and deployment for ${env.JOB_NAME} #${env.BUILD_NUMBER} was successful."
                              def payload = """{
                                   "text": "${message_success}"
                              }"""
                         //slackSend(channel: SLACK_CHANNEL, message: "Deployment Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                     } else {
                         echo 'Sending failure notifications...'
                         mail to: 'lr_gueddouche@esi.dz',
                              subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                              body: "The build for ${env.JOB_NAME} #${env.BUILD_NUMBER} failed. Check the logs for details."
                              def payload = """{
                                   "text": "${message}"
                              }"""
                         //slackSend(channel: SLACK_CHANNEL, message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                     }
                      def connection = new URL(webhookUrl).openConnection() as HttpURLConnection
                         connection.requestMethod = 'POST'
                         connection.doOutput = true
                         connection.setRequestProperty('Content-Type', 'application/json')

                         connection.outputStream.withWriter('UTF-8') { writer ->
                             writer.write(payload)
                         }
                 }
             }
         }

    }
}