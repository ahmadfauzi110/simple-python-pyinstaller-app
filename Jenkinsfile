pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Manual Approval'){
            steps {
                script {
                    def userInput = input(
                        id: 'ProceedOrAbort',
                        message: 'Lanjutkan ke tahap Deploy?',
                        parameters: [
                            choice(name: 'ACTION', choices: ['Proceed', 'Abort'], description: 'Pilih aksi')
                        ]
                    )

                    if (userInput == 'Abort') {
                        error("Pipeline dihentikan oleh user.")
                    }
                }
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python source/add2val.py 8 7'
                sleep(60)
            }
        }
    }
    
}