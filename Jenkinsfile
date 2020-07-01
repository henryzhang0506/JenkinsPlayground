pipeline {
    agent any
    environment {
        CXX = 'g++'
        HENRY= 'great'
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                python --version
                ls -lha
                printenv
                '''
                sh 'echo test'
                input 'Does the staging environment look ok?'
            }
        }
        stage('Test') {
            steps {
                sh 'sh -x ./test.sh || true'
            }
        }
        stage('Deploy') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                }
            }
            steps {
                retry(3) {
                    sh 'sh -x ./flakey-deploy.sh'
                }

                timeout(time: 3, unit: 'MINUTES') {
                    sh 'sh -x ./health-check.sh'
                }
            }
        }
    }
}
