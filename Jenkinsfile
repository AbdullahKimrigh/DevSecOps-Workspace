pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_S3_BUCKET = 'scanreport-bucket'
        ARTIFACT_NAME = 'testreport.html'
    }

    stages {
        stage('Run Zap Scan') {
            steps {
                sh 'docker run -d -v $(pwd):/zap/wrk/:rw -t owasp/zap2docker-stable zap-full-scan.py -t http://ec2-54-91-236-219.compute-1.amazonaws.com/ -g gen.conf -r testreport.html -z "-config scanner.strength=Medium"'
            }
        }

        stage('Publish Report') {
            steps {
                echo "suuccess"            
            }
        }
    }
}