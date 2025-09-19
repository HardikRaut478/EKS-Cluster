pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION    = "ap-south-1"
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
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
                sh "terraform plan"
            }
        }

        stage("Terraform Action") {
            steps {
                script {
                    switch (params.ACTION) {
                        case 'apply':
                            echo 'Executing Apply...'
                            sh "terraform apply --auto-approve"
                            break
                        case 'destroy':
                            echo 'Executing Destroy...'
                            sh "terraform destroy --auto-approve"
                            break
                        default:
                            error 'Unknown action'
                    }
                }
            }
        }
    }
}
