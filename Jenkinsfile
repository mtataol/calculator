pipeline {
    agent any
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                 sh "./gradlew test"
            }
        }
        stage("Code coverage") {
            steps {
                 sh "./gradlew jacocoTestReport"
                 publishHTML (target: [
                     reportDir: 'build/reports/jacoco/test/html',
                     reportFiles: 'index.html',
                     reportName: "JaCoCo Report"
                 ])
                 sh "./gradlew jacocoTestCoverageVerification"
            }
        }
        stage("Static code analysis") {
            steps {
                 sh "./gradlew checkstyleMain"
                 publishHTML (target: [
                     reportDir: 'build/reports/checkstyle/',
                     reportFiles: 'main.html',
                     reportName: "Checkstyle Report"
                 ])
            }
        }
    }
    post {
        always {
            emailext to: "tamer@robolaunch.cloud",
            subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
            body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
            attachmentsPattern: '*.html'
        }
    }
}

