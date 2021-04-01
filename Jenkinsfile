pipeline {
    agent any
    stages {
        stage('Stage 1') {
            steps {
                git branch: 'fc',
                    url: 'https://github.com/sushantadas90/external.git'
            }
        }
        stage('stage 2') {
            environment {
                PORT = 8081
            }
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('stage 3') {
            steps {
                sh 'gcloud builds submit --tag gcr.io/positive-tempo-309316/external-image:v${BUILD_NUMBER} .'
            }
        }

        stage('stage 4') {
            steps {
                sh 'gcloud container clusters get-credentials devops-cluster --zone us-central1-c --project positive-tempo-309316'
                sh 'kubectl set image deployment/events-external external-image=gcr.io/positive-tempo-309316/external-image:v${BUILD_NUMBER} --record -n development'
            }
        }
    }

}
