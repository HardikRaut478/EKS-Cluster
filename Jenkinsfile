pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "ap-south-1"
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')         // Jenkins Credentials ID
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key') // Jenkins Credentials ID
    }

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Select the action to perform'
        )
    }

    stages {
        stage("Terraform Init") {
            steps {
                sh "terraform init -reconfigure"
            }
        }

        stage("Terraform Plan") {
            steps {
                sh "terraform plan -out=tfplan"
            }
        }

        stage("Terraform Action") {
            steps {
                script {
                    if (params.ACTION == 'apply') {
                        echo "Running terraform apply..."
                        sh "terraform apply --auto-approve tfplan"
                    } else if (params.ACTION == 'destroy') {
                        echo "Running terraform destroy..."
                        sh "terraform destroy --auto-approve"
                    } else {
                        error "Unknown action"
                    }
                }
            }
        }
    }
}
