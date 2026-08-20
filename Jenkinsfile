pipeline {
    agent {
        docker {
            image 'postman/newman:latest'
            args '-u root --entrypoint='
        }
    }

    stages {

        stage('Lancer les tests') {
            steps {
                sh 'newman run collection.json -e preprod.json'
            }
        }
    }
}
