pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
		sh './gradlew clean build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './gradlew check'
                sh './gradlew jacocoTestReport '

                // Publis the JUnit test Report
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports/tests/test',
                    reportFiles: 'index.html',
                    reportName: 'JUnit Report'
                  ]

                // Publish the Java Code Coverage Report
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/jacocoHtml',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Report'
                  ]
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh './gradlew jar'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: 'build/libs/**/*.war', fingerprint: true
            junit 'gradle-java-at08/build/test-results/**/*.xml'
        }
    }
}