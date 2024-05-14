pipeline {
    agent any

    environment {
        ALLURE_RESULTS_DIR = 'target/allure-results'
    }

    options {
        // Ensure the workspace is cleaned up before starting the build
        skipDefaultCheckout true
    }

    stages {


        stage('Build and Test') {
            steps {
                script {
                    // Run Maven build and tests
                    try {
                        sh 'mvn clean test'
                    } catch (Exception e) {
                        // Mark the build as unstable if tests fail
                        currentBuild.result = 'UNSTABLE'
                        echo 'Some tests failed, marking the build as unstable.'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Archive the Allure results even if the build fails or is unstable
                allure includeProperties: false, jdk: '', results: [[path: "${ALLURE_RESULTS_DIR}"]]
            }
        }

        success {
            script {
                // Perform any additional actions on success
                echo 'Build succeeded, Allure report generated.'
            }
        }

        unstable {
            script {
                // Perform any additional actions if the build is unstable
                echo 'Build unstable, Allure report generated with test failures.'
            }
        }

        failure {
            script {
                // Handle failure case
                echo 'Build failed, check the logs for details.'
            }
        }

        cleanup {
            script {
                // Clean up the workspace after the build
                cleanWs()
            }
        }
    }
}
