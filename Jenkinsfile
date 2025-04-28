pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'               // Change as needed
        INSTANCE_ID = 'i-09b3b59bb7dd7cbd7'    // Replace with your EC2 instance ID
    }

    parameters {
        choice(name: 'ACTION', choices: ['start', 'stop'], description: 'Choose action to perform on EC2 instance')
    }

    stages {
        stage('Execute EC2 Action') {
            steps {
                script {
                    if (params.ACTION == 'start') {
                        sh """
                        aws ec2 start-instances --instance-ids $INSTANCE_ID --region $AWS_REGION
                        echo "Waiting for instance to start..."
                        aws ec2 wait instance-running --instance-ids $INSTANCE_ID --region $AWS_REGION
                        echo "Instance started."
                        """
                    } else if (params.ACTION == 'stop') {
                        sh """
                        aws ec2 stop-instances --instance-ids $INSTANCE_ID --region $AWS_REGION
                        echo "Waiting for instance to stop..."
                        aws ec2 wait instance-stopped --instance-ids $INSTANCE_ID --region $AWS_REGION
                        echo "Instance stopped."
                        """
                    } else {
                        error("Invalid action: ${params.ACTION}")
                    }
                }
            }
        }
    }
}
