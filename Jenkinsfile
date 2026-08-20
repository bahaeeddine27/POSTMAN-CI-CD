pipeline{
    agent any
 
    stages{
        stage('global stage'){
            agent{
                docker{
                    image 'postman/newman:latest'
                    args '-u root --entrypoint='
                }
            }
            stages{
                stage('Installer Allure') {
                        steps {
                            sh 'npm install -g newman-reporter-allure'
                        }
                }
                stage('clean allure results'){
                    
                    steps{
                        sh '''
                            echo "Suppression du cache Allure..."
                            rm -rf allure-results
                            mkdir -p allure-results
                            echo "Dossier allure-results nettoyé avec succès"
                        '''
                    }
                }
        
                stage('run user test'){
                    steps{  
                        sh"newman run collection.json -e preprod.json --reporters cli,allure --reporter-allure-resultsDir allure-results"
                        stash name: 'allure-results', includes: 'allure-results/*'
                    }
                }
            }
        }
    }
    post{
        always{
                    unstash 'allure-results'
                    archiveArtifacts 'allure-results/*'
                    allure includeProperties: false,
                           jdk: '',
                           results: [[path: 'allure-results/']]
        }
    }
} 