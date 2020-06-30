pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh '''
                python --version
                ls -lha
                '''
                sh 'echo test'
            }
        }
        stage('Deploy') {
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
