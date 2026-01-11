pipeline {
    agent any

    stages {

        stage('Pre-Validation') {
            steps {
                echo "Pipeline started"
            }
        }

        stage('Manual Approval for PROD') {
            steps {
                input message: 'Approve deployment to PROD?', ok: 'Deploy'
            }
        }

        stage('Critical Operation') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    sh '''
                    echo "Running critical operation"
                    exit 1
                    '''
                }
            }
        }

        stage('Post-Critical Steps') {
            steps {
                echo "Pipeline continues even after failure"
            }
        }
    }

    post {
        always {
            echo "Cleanup & reporting executed"
        }
        unstable {
            echo "Build marked as UNSTABLE due to controlled failure"
        }
        success {
            echo "Build successful"
        }
    }
}
