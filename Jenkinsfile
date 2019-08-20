#!groovy
pipeline {
    agent any
    parameters {
        choice(choices: ['create', 'delete'], description: 'create or delete stack', name: 'action')
    }
    stages {
		stage('Code checkout') {
			steps {
				deleteDir()
				checkout scm
            }
        }

        stage('Create stack') {
            when {
                expression { params.action == 'create' }
            }
			steps {
                sh '''aws cloudformation deploy --template-file ECS-stack.yaml \
                    --stack-name "ECS-stack" \
                    --capabilities=CAPABILITY_NAMED_IAM \
                    --region us-east-1'''
            }
        }

        stage('Delete stack') {
            when {
                expression { params.action == 'delete' }
            }
			steps {
                sh '''aws cloudformation delete-stack --stack-name ECS-stack'''
            }
        }
    }
}