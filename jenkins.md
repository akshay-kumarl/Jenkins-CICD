# Complicated sample Jenkinsfile


```
pipeline {
    agent any
   // agent { label 'slave-1'}

    tools {
        terraform 'terraform_1.9.6'  // Name from the Global Tool Configuration
    }

    environment {
        AWS_CREDENTIALS_ID = 'aws-credentials'   // ID of the AWS credentials stored in Jenkins
        TF_WORKSPACE = 'terraform-workspace'     // Workspace for Terraform files
    }

    stages {
        stage('Checkout') {
            steps {
                // Assuming the Terraform script is in the repository
                git branch: 'main', url: 'https://github.com/your-repo/terraform-aws-ec2.git'
            }
        }
        stage('Init Terraform') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: AWS_CREDENTIALS_ID]]) {
                    sh 'terraform init'
                }
            }
        }
        stage('Plan Terraform') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: AWS_CREDENTIALS_ID]]) {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }
        stage('Apply Terraform') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: AWS_CREDENTIALS_ID]]) {
                    // Apply the Terraform plan
                echo "Terraform action is --> ${action}"
                sh ('terraform ${action} --auto-approve tfplan') 
                }
            }
        }
    }

    post {
        always {
            echo "Terraform operation complete!"
        }
    }
}
```
