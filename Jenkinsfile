#!groovy

pipeline {
    agent any
    environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
      }

    //tools {
        // Install the Maven version configured as "M3" and add it to the path.
       // maven "M3"
    //}

    stages {
        stage("Initialization") {
            steps {
                // use name of the build name
                buildName "${appName}-#${BUILD_NUMBER}"
                buildDescription "Executed ${BUILD_NUMBER}"
            }
        }
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/ashok2495/${appName}.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            //post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                //success {
                    //junit '**/target/surefire-reports/TEST-*.xml'
                    //archiveArtifacts 'target/*.jar'
                //}
            //}
        }
        stage('Deploy to Cloudhub') {
            steps {
                 withCredentials([usernamePassword(credentialsId: 'anypoint.credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER')]) {
                sh 'mvn deploy -P cloudhub-dev -Dmule.version=4.3.0 -Danypoint.username=$USER -Danypoint.password=$PASSWORD'
                echo 'SUCCESS'
                 }
            }
        }
    }
}
