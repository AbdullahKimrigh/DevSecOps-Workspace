pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_S3_BUCKET = 'scanreport-bucket'
        ARTIFACT_NAME = 'testreport.html'
        
        INSTANCE_DNS = "ec2-54-91-236-219.compute-1.amazonaws.com"
    }

    stages {
         stage('Run App') {
            steps {
                sh 'docker run -d -t vulnerables/web-dvwa .'
            }
        }

        stage('Run Zap Scan') {
            steps {
                sh 'docker run -v $(pwd):/zap/wrk/:rw -t owasp/zap2docker-stable zap-full-scan.py -t http://$INSTANCE_DNS -g gen.conf -r testreport.html -z "-config scanner.strength=Medium"'
            }
            
            post {
                success {
                    archiveArtifacts artifacts: 'testreport.html', followSymlinks: false                  
                }
            }
        }

        stage('Publish Artifacts') {
            steps {
                sh "aws configure set region us-east-1"
                sh "aws s3 cp testreport.html s3://$AWS_S3_BUCKET/$ARTIFACT_NAME"               
            }
        }
    }
}