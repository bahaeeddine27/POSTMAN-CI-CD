pipeline {
    agent none

    stages {

        stage('Lancer les tests API') {
            agent {
                docker {
                    image 'postman/newman:latest'
                    args '-u root --entrypoint='
                }
            }

            steps {
                sh '''
                    npm install -g newman-reporter-allure

                    mkdir -p /allure-report

                    newman run collection.json \
                    -e preprod.json \
                    -r cli,allure \
                    --reporter-allure-resultsDir /allure-report
                '''
            }
        }

        stage('Générer le rapport Allure') {
            agent any

            steps {
                allure([
                    includeProperties: false,
                    jdk: '',
                    results: [[path: '/allure-report']]
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '/allure-report/**',
                             allowEmptyArchive: true
        }
    }
}