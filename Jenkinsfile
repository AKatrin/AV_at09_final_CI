pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Compiling..'
                sh './sampleWebApp/gradlew assemble -p sampleWebApp/'
                archiveArtifacts 'sampleWebApp/build/libs/*.jar'
            }
        }
        stage('Unit Test') {
            steps {
                echo 'Executing Unit Tests 1 ..'
                sh './sampleWebApp/gradlew test -p sampleWebApp/'
            }
            post {
                always {
                    junit "sampleWebApp/build/test-results/test/*.xml"
                    archiveArtifacts 'sampleWebApp/build/reports/tests/test/*'
                }       
            }
        }
        stage('Sonarqube') {
            steps {
                echo 'Executing Sonarqube ..'
                sh './sampleWebApp/gradlew sonarqube -p sampleWebApp/'
            }
        }
    }
    post {
        success {
            mail to: 'areliez.vargas@gmail.com',
                subject: "Success Pipeline: ${currentBuild.fullDisplayName}",
                body: "The build was successful: ${env.BUILD_URL}"
        }
    }
}
