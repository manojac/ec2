pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'               // Change to your region
        INSTANCE_ID = 'i-07f3fca583c2da33d'    // Replace with your EC2 instance ID
        SLEEP_TIME = 200                       // Sleep time in seconds (e.g., 300 = 5 minutes)
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                echo "Starting EC2 instance: ${env.INSTANCE_ID}"
                sh """
                    aws ec2 start-instances --instance-ids $INSTANCE_ID --region $AWS_REGION
                    echo "Waiting for instance to enter 'running' state..."
                    aws ec2 wait instance-running --instance-ids $INSTANCE_ID --region $AWS_REGION
                    echo "Instance is now running."
                """
            }
        }

        stage('Wait (Sleep)') {
            steps {
                echo "Sleeping for ${env.SLEEP_TIME} seconds to simulate usage time..."
                sleep time: env.SLEEP_TIME.toInteger(), unit: 'SECONDS'
            }
        }

        stage('Stop EC2 Instance') {
            steps {
                echo "Stopping EC2 instance: ${env.INSTANCE_ID}"
                sh """
                    aws ec2 stop-instances --instance-ids $INSTANCE_ID --region $AWS_REGION
                    echo "Waiting for instance to enter 'stopped' state..."
                    aws ec2 wait instance-stopped --instance-ids $INSTANCE_ID --region $AWS_REGION
                    echo "Instance is now stopped."
                """
            }
        }
    }
}
