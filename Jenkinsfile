pipeline {
    agent none
    environment {
        creds = credentials('get-share')
        project_name = ('env.JOB_NAME')
    }
    stages {
        stage('Run Task on 1st slave') {
            agent {
                    label 'win_slaves'
                }
            steps {
                script {
                    FAILED_STAGE=env.STAGE_NAME
                }
                bat """
                    cd C:\\script
                    python test_share.py -u %creds_usr% -p %creds_psw% -path %location% -pname %project_name%
                """    
            }
        }
        stage('Run Task on 2nd slave') {
            agent {label 'win_slave2'
                }
            steps {
                script {
                    FAILED_STAGE=env.STAGE_NAME
                }
                bat """
                    net use T: \\\\192.168.0.13\\TestShare /u:%creds_usr% %creds_psw%
                    python T:\\Hello.py
                """    
            }
        }
        stage('Run Task on 1st slave again') {
            agent {label 'win_slaves'
                }
            steps {
                script {
                    FAILED_STAGE=env.STAGE_NAME
                }
                bat """
                    dir C:\\Copy
                """
            }
        }
    }
    post {
        failure {
            echo "Failed job: $FAILED_STAGE"
            }
        success {
            echo "Finished successfully!"
            }
        }
}
