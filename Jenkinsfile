pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['DEV', 'PROD'],
            description: 'Select environment'
        )
    }

    environment {
        APP_NAME = "DiskMonitor"
        DISK_LIMIT = "80"
    }

    stages {

        stage('Pre Check') {
            steps {
                echo "Starting ${APP_NAME} pipeline"
                echo "Environment selected: ${params.ENVIRONMENT}"
            }
        }

        stage('Disk Usage Check') {
            when {
                expression { params.ENVIRONMENT == 'PROD' }
            }
            options {
                timeout(time: 1, unit: 'MINUTES')
            }
            steps {
                retry(2) {
                    sh '''
                    usage=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
                    echo "Current Disk Usage: $usage%"
                    if [ $usage -gt $DISK_LIMIT ]; then
                        echo "Disk usage exceeded limit"
                        exit 1
                    else
                        echo "Disk usage under control"
                    fi
                    '''
                }
            }
        }

        stage('DEV Skip Message') {
            when {
                expression { params.ENVIRONMENT == 'DEV' }
            }
            steps {
                echo "Disk check skipped in DEV environment"
            }
        }
    }

    post {
        success {
            echo "✅ ${APP_NAME} pipeline SUCCESS"
        }
        failure {
            echo "❌ ${APP_NAME} pipeline FAILED"
        }
        always {
            echo "🔔 Pipeline execution completed"
        }
    }
}
