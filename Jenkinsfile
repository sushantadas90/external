pipeline {
    agent any
    stages {
        stage('Stage 1') {
            steps {
                git branch: 'fc',
                    url: 'https://github.com/lahcsin/external.git'
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
                sh 'gcloud builds submit --tag gcr.io/mar-roidtc503/external-image:v${BUILD_NUMBER} .'
            }
        }

        stage('stage 4') {
            steps {
                sh 'gcloud container clusters get-credentials devops-cluster --zone us-central1-c --project mar-roidtc503'
                sh 'kubectl set image deployment/events-external external-image=gcr.io/mar-roidtc503/external-image:v${BUILD_NUMBER} --record -n development'
            }
        }
    }

}
