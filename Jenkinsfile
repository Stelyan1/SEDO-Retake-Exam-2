pipeline {
    agent any

    triggers {
        // стартира при промени в main (ако имаш GitHub webhook, иначе може да остане pollSCM)
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --configuration Release --no-build --verbosity normal --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            // публикува тестови резултати, ако има .trx
            junit '**/TestResults/*.xml'
        }
    }
}
