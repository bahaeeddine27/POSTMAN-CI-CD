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

                    mkdir -p build/allure-results

                    newman run collection.json \
                    -e preprod.json \
                    -r cli,allure \
                    --reporter-allure-resultsDir build/allure-results
                '''
            }
        }

        stage('Générer le rapport Allure') {
            agent any

            steps {
                allure([
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'build/allure-results']]
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/allure-results/**',
                             allowEmptyArchive: true
        }
    }
}