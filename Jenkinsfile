pipeline {
    agent {
        docker {
            image 'postman/newman:latest'
            args '-u root --entrypoint='
        }
    }

    stages {

        stage('Lancer les tests API') {
            steps {
                sh '''
                    newman run collection.json \
                    -e preprod.json \
                    -r cli,allure \
                    --reporter-allure-resultsDir build/allure-results
                '''
            }
        }
    }

    post {
        always {
            allure includeProperties: false,
                   jdk: '',
                   results: [[path: 'build/allure-results']]
        }
    }
}