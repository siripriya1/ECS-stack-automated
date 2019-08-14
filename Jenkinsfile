#!groovy
pipeline {
    agent any
    stages {
		stage('Code checkout') {
			steps {
				deleteDir()
				checkout scm
            }
        }

        stage('Create stack') {
			steps {
                sh '''aws cloudformation deploy --template-file ECS-stack.yaml \
                    --stack-name "ECS-stack" \
                    --capabilities=CAPABILITY_NAMED_IAM \
                    --region us-east-1'''
            }
        }
    }
}