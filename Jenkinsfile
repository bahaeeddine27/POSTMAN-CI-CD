pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile'
            args '-u root'
        }
    }

    stages {

        stage('Lancer les tests API') {
            steps {
                sh '''
                    newman run collection.json \
                    -e preprod.json \
                    -r cli,allure \
                    --reporter-allure-export allure-results
                '''
            }
        }
    }

    post {
        always {
            allure([
                includeProperties: false,
                jdk: '',
                results: [[path: 'allure-results']]
            ])
        }
    }
}