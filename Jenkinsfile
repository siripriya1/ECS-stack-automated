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
                //Custodian commands already executed.
                //custodian validate ec2Stop.yaml
                //custodian run --dryrun -s . --region us-east-1 ec2Stop.yaml
                //custodian run -s . --region us-east-1 ec2Stop.yaml
                sh '''aws cloudformation create-stack \
                    --stack-name "${TARGET_STACK_NAME}" \
                    --template-body file://ecs-${CONTAINER_NAME}.yaml \
                    --parameters \
                        "ParameterKey=ServicesCluster,ParameterValue=${TARGET_CLUSTER}" \
                        "ParameterKey=ServiceName,ParameterValue=${SERVICE_NAME}" \
                        "ParameterKey=ContainerName,ParameterValue=${CONTAINER_NAME}" \
                        "ParameterKey=ContainerTag,ParameterValue=${TAG_NAME}" \
                        "ParameterKey=TargetGroup,ParameterValue=${TARGET_GROUP}" \
                        "ParameterKey=CloudWatchLogsGroup,ParameterValue=${CLOUD_WATCH_LOGS_GROUP}" \
                    --tags \
                        "Key=KS,Value=True" \
                        "Key=StackPrefix,Value=${IMAGE_NAME}" \
                        "Key=StackType,Value=${DEPLOY_ENV}" \
                        "Key=CanPromote,Value=true" \
                    --timeout-in-minutes 10  '''
            }
        }
    }
}